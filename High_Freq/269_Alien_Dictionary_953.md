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

# Solution: Topology Sort (Beat 100%)
```
class Solution {
    public String alienOrder(String[] words) {
        boolean[] dict = new boolean[26];
        if(words.length == 1) {
            for(int i = 0; i < words[0].length(); i++) dict[words[0].charAt(i) - 'a'] = true;
            return topology_sort(new boolean[26][26],new int[26],dict);
        }
        boolean[][] map = new boolean[26][26];
        int[] indegree = new int[26];
        
        
        for(int i = 1; i < words.length; i++){
            String word1 = words[i-1];
            String word2 = words[i];
            if(!adding_links_to(map,indegree,dict,word1,word2)) return new String();
        }
        
        return topology_sort(map,indegree,dict);
    }
    
    public String topology_sort(boolean[][] map, int[] indegree, boolean[] dict){
        StringBuilder sb = new StringBuilder();
        int cnt = 0;
        for(int i = 0; i < 26; i++){
            cnt += indegree[i];
            if(indegree[i] == 0 && dict[i] == true){
                sb.append((char)(i+'a'));
                dict[i] = false;
                remove_links(i, map, indegree);
                i = -1;cnt = 0;
            }
        }
        
        
        if(cnt != 0) return new String();
        
        for(int i = 0; i < 26; i++){
            if(dict[i] == true) sb.append((char)(i+'a'));
        }
        
        return sb.toString();
    }
    
    public void remove_links(int index, boolean[][] map, int[] indegree){
        for(int i = 0;  i< 26; i++){
            if(map[index][i] == true){
                indegree[i]--;
            }
        }
    }
    
    
    
    public boolean adding_links_to(boolean[][] map, int[] indegree, boolean[] dict, String word1, String word2){
        int ptr1 = 0, ptr2 = 0;
        while(ptr1 < word1.length() && ptr2 < word2.length() && word1.charAt(ptr1) == word2.charAt(ptr2)){
            dict[word1.charAt(ptr1)-'a'] = true;
            ptr1++; ptr2++;    
        }
        
        if(ptr1 < word1.length() && ptr2 == word2.length()) return false;
        
        if(ptr1 < word1.length() && ptr2 < word2.length()){
            if(!map[word1.charAt(ptr1)-'a'][word2.charAt(ptr2)-'a']) indegree[word2.charAt(ptr2)-'a']++;
            map[word1.charAt(ptr1)-'a'][word2.charAt(ptr2)-'a'] = true;
        }
        
        
        while(ptr1 < word1.length()){
            dict[word1.charAt(ptr1)-'a'] = true;
            ptr1++;
        }
        
        while(ptr2 < word2.length()){
            dict[word2.charAt(ptr2)-'a'] = true;
            ptr2++;
        }
        
        return true;
    }
}
```

Better Coding:
```
class Solution {
    public String alienOrder(String[] words) {
        HashSet<Character> set = new HashSet<Character>();
        StringBuilder sb = new StringBuilder();
        boolean[][] graph = new boolean[26][26];
        int[] in_degree = new int[26];
        
        if(words.length == 1){
            for(char c: words[0].toCharArray()) set.add(c);
        }
        
//         add relationship
        for(int i = 0; i < words.length - 1; i++){
            char[] word1 = words[i].toCharArray();
            char[] word2 = words[i+1].toCharArray();
            
            int j = 0;
            while(j < word1.length && j < word2.length && word1[j] == word2[j]){
                set.add(word1[j++]);
            }
            
            if(j < word1.length && j == word2.length) return "";
            if(j < word1.length && j < word2.length && !graph[word1[j]-'a'][word2[j]-'a']){
                graph[word1[j]-'a'][word2[j]-'a'] = true;
                in_degree[word2[j]-'a']++;
            }
            
            int tmpj = j;
            while(j < word1.length) set.add(word1[j++]);
            j = tmpj;
            while(j < word2.length) set.add(word2[j++]);
        }
        
//         Topology sort: find the elements that in_degree equals to 0;
        while(!set.isEmpty()){
            char c = '0';
            int index = -1;
            for(int i = 0; i < 26; i++){
                if(in_degree[i] == 0 && set.contains((char)(i+'a'))){
                    c = (char)(i+'a');
                    index = i;
                    set.remove(c);
                    break;
                }
            }
            if(index == -1) return "";
            sb.append(c);
            for(int i = 0; i < 26; i++){
                if(graph[index][i]){
                    graph[index][i] = false;
                    in_degree[i]--;
                }
            }
        }
        
        
        return sb.toString();
    }
}
```