# 438. Find All Anagrams in a String 567 242 76
Given a string  **s**  and a  **non-empty**  string  **p**, find all the start indices of  **p**'s anagrams in  **s**.

Strings consists of lowercase English letters only and the length of both strings  **s**  and  **p**  will not be larger than 20,100.

The order of output does not matter.

**Example 1:**

**Input:**
s: "cbaebabacd" p: "abc"

**Output:**
[0, 6]

**Explanation:**
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".

**Example 2:**

**Input:**
s: "abab" p: "ab"

**Output:**
[0, 1, 2]

**Explanation:**
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".

# Solution 1: Sliding Window
Easier to understand:
```
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        LinkedList<Integer> res = new LinkedList<Integer>();
        if(s.length() < p.length()) return res;
        int[] map = new int[26];
        for(int i = 0; i < p.length(); i++){
            map[p.charAt(i) - 'a'] ++;
            map[s.charAt(i) - 'a'] --;
        }
        if(isAllZeros(map)) res.add(0);
        
        int len = p.length();
        for(int i = len; i <s.length(); i++){
            map[s.charAt(i) - 'a'] --;
            map[s.charAt(i- len) - 'a'] ++;
            if(isAllZeros(map)) res.add(i-len+1);
        }
        
        return res;
    }
    
    public boolean isAllZeros(int[] map){
        for(int i = 0; i<map.length; i++){
            if(map[i] != 0) return false;
        }
        return true;
    }
    
}
```

More complicated 
```
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        LinkedList<Integer> res = new LinkedList<Integer>();
        int[] map = new int[26];
        popular_map(p,map);
        
        int start = 0;
        int end = 0;
        int unmatched = p.length();
        
        while(end < s.length()){
            if(map[s.charAt(end) - 'a'] > 0) unmatched--;
            map[s.charAt(end) - 'a']--;
            if(unmatched == 0) res.add(start);
            end++;
            if(end-start == p.length()){
                if(map[s.charAt(start) - 'a'] >= 0) unmatched++;
                map[s.charAt(start) - 'a']++;
                start++;
            }
            
        }
        
        return res;
    }
    
    public void popular_map(String p, int[] map){
        for(int i = 0; i < p.length(); i++){
            map[p.charAt(i)-'a']++;
        }
    }
    
}
```