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

# Solution: Topology Sort (Beat 18%)
```
class Solution {
    public String alienOrder(String[] input) {        
        if(input.length == 1) return input[0];
        
        int[] count = new int[26];
        for(int i = 0; i < count.length; i++) count[i] = -1;
        LinkedList<Integer>[] children = new LinkedList[26];
        int total_letter = 0;
        
        for(int i = 0; i < input.length - 1; i++){
            char[] word1 = input[i].toCharArray();
            char[] word2 = input[i+1].toCharArray();
            
            for(char ch : word1){
                if(count[ch-'a'] == -1) {
                    count[ch-'a'] = 0;
                    total_letter++;
                    children[ch-'a'] = new LinkedList<Integer>();
                }
            }
            
            for(char ch : word2){
                if(count[ch-'a'] == -1) {
                    count[ch-'a'] = 0;
                    total_letter++;
                    children[ch-'a'] = new LinkedList<Integer>();
                }
            }
            
            for(int j = 0; j < word1.length && j < word2.length; j++){
                if(word1[j] != word2[j]){
                    children[word1[j]-'a'].add(word2[j] - 'a');
                    count[word2[j]-'a']++; 
                    break;
                }
                if(j == word2.length-1 && j < word1.length - 1) return new String("");
            }
            
        }
        
//         topology sort
        StringBuilder sb = new StringBuilder();
        Queue<Integer> q = new LinkedList<Integer>();
        
        for(int i = 0; i < count.length; i++) if(count[i] == 0) q.offer(i);
        while(!q.isEmpty()){
            char x = (char)(q.poll() + (int)'a');
            sb.append(x + "");
            total_letter--;
            for(Integer ch: children[x-'a']){
                count[ch]--;
                if(count[ch] == 0) q.offer(ch);
            }
        }    
        return total_letter == 0?sb.toString(): new String("");
    }
   
}
```