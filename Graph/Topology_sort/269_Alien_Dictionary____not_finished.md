# 269. Alien Dictionary
There is a new alien language which uses the latin alphabet. However, the order among letters are unknown to you. You receive a list of **non-empty** words from the dictionary, where **words are sorted lexicographically by the rules of this new language**. Derive the order of letters in this language.

**Example 1:**

**Input:**
[
  "wrt",
  "wrf",
  "er",
  "ett",
  "rftt"
]

**Output:** `"wertf"`

**Example 2:**

**Input:**
[
  "z",
  "x"
]

**Output:** `"zx"`

**Example 3:**

**Input:**
[
  "z",
  "x",
  "z"
] 

**Output:** `""` 

**Explanation:** The order is invalid, so return `""`.

**Note:**

-   You may assume all letters are in lowercase.
-   If the order is invalid, return an empty string.
-   There may be multiple valid order of letters, return any one of them is fine.
# Solution: 
```
class Solution {
    public String alienOrder(String[] input) {        
        if(input.length == 1) return input[0];
        HashMap<Character, HashSet<Character>> parents = new HashMap<Character, HashSet<Character>>();
		
        for(int i = 0; i < input.length - 1; i++){
            char[] word1 = input[i].toCharArray();
            char[] word2 = input[i+1].toCharArray();
            
            for(char ch : word1){
                if(!parents.containsKey(ch))parents.put(ch, new HashSet<Character>());
            }
            
            for(char ch : word2){
                if(!parents.containsKey(ch))parents.put(ch, new HashSet<Character>());
            }
            
            for(int j = 0; j < word1.length && j < word2.length; j++){
                if(word1[j] != word2[j]){
                    HashSet<Character> set = parents.get(word2[j]);
                    set.add(word1[j]);
                    parents.put(word2[j],set);
                    j = word1.length;
                }
                if(j == word2.length-1 && j < word1.length - 1) return new String("");
            }
            
        }

        StringBuilder sb = new StringBuilder();
//         topology sort
        while(parents.size() != 0){
            char x = find_zero_indegree(parents);
            if(x == '#') return new String("");
            sb.append(x + "");
            remove_from_map(parents,x);
        }    
        return sb.toString();
    }
    
    
    public void remove_from_map(HashMap<Character, HashSet<Character>> parents, char x){
        parents.remove(x);
        for(Character key : parents.keySet()) 
            parents.get(key).remove(x);
    }
    
    public char find_zero_indegree(HashMap<Character, HashSet<Character>> parents){
        for(Character key : parents.keySet()) 
            if(parents.get(key).size() == 0) return key;
        return '#';
    }
}
```