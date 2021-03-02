# 267. Palindrome Permutation II
Given a string  `s`, return all the palindromic permutations (without duplicates) of it. Return an empty list if no palindromic permutation could be form.

**Example 1:**

**Input:** `"aabb"`
**Output:** `["abba", "baab"]`

**Example 2:**

**Input:** `"abc"`
**Output:** `[]`


# Solution 1: HashMap + HashSet + BackTracking (Beat 88%)
```
class Solution {
    
    List<String> res = new ArrayList<String>();
    char[] word;
    Set<Character> keyset;
    public List<String> generatePalindromes(String s) {
        word = s.toCharArray();
        HashSet<Character> set = new HashSet<Character>();
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        
        for(char c: word){
            if(!set.contains(c)) set.add(c);
            else {
                map.put(c, map.getOrDefault(c,0) + 1);
                set.remove(c);
            }
        }
        
        if(set.size() > 1) return res;
        
        if(set.size() == 1) {
            char middle = ' ';
            for(char c: set) middle = c;
            word[s.length()/2] = middle;
        }
        
        keyset = map.keySet();
        dfs(map,0,s.length()-1);
    
        return res;
    }
    
    public void dfs(HashMap<Character, Integer> map, int left, int right){
        if(left == word.length/2){
            res.add(new String(word));
            return;
        }
        
		for(Character key : keyset) {
			if(map.get(key) == 0) continue;
            map.put(key, map.get(key)-1);
            word[left] = key;
            word[right] = key;
            dfs(map,left+1,right-1);
            map.put(key, map.get(key)+1);
		}
    }
 
}
```