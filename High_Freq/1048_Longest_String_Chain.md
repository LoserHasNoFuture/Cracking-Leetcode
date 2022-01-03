# 1048. Longest String Chain
You are given an array of  `words`  where each word consists of lowercase English letters.

`wordA`  is a  **predecessor**  of  `wordB`  if and only if we can insert  **exactly one**  letter anywhere in  `wordA`  **without changing the order of the other characters**  to make it equal to  `wordB`.

-   For example,  `"abc"`  is a  **predecessor**  of  `"abac"`, while  `"cba"`  is not a  **predecessor**  of  `"bcad"`.

A  **word chain**  is a sequence of words  `[word1, word2, ..., wordk]`  with  `k >= 1`, where  `word1`  is a  **predecessor**  of  `word2`,  `word2`  is a  **predecessor**  of  `word3`, and so on. A single word is trivially a  **word chain**  with  `k == 1`.

Return  _the  **length**  of the  **longest possible word chain**  with words chosen from the given list of_ `words`.

**Example 1:**

**Input:** words = ["a","b","ba","bca","bda","bdca"]
**Output:** 4
**Explanation**: One of the longest word chains is ["a","ba","bda","bdca"].

**Example 2:**

**Input:** words = ["xbc","pcxbcf","xb","cxbc","pcxbc"]
**Output:** 5
**Explanation:** All the words can be put in a word chain ["xb", "xbc", "cxbc", "pcxbc", "pcxbcf"].

**Example 3:**

**Input:** words = ["abcd","dbqca"]
**Output:** 1
**Explanation:** The trivial word chain ["abcd"] is one of the longest word chains.
["abcd","dbqca"] is not a valid word chain because the ordering of the letters is changed.

**Constraints:**

-   `1 <= words.length <= 1000`
-   `1 <= words[i].length <= 16`
-   `words[i]`  only consists of lowercase English letters.

# Solution 1: DFS (Beat 7%)
```
class Solution {
    int[] memo;
    public int longestStrChain(String[] words) {
        Arrays.sort(words, (a,b)->(a.length()-b.length()));
        HashMap<String, Integer> map = new HashMap<String, Integer>();
        int n = words.length, res = 0;
        boolean[][] graph = new boolean[n][n];
        boolean[] has_indegree = new boolean[n], visited = new boolean[n];
        memo = new int[n];
        
        for(int i = 0; i < n; i++){    
            for(int j = 0; j < words[i].length(); j++){
                String str = words[i].substring(0, j) + words[i].substring(j+1);
                if(map.containsKey(str)) {
                    int index = map.get(str);
                    graph[i][index] = true;    
                    has_indegree[index] = true;
                }
            }
            map.put(words[i], i);
        }
        
        
        
        for(int i = 0; i < n; i++){
            if(!has_indegree[i]) dfs(graph, visited, i);
        }
        
        for(int i = 0; i < n; i++) res = Math.max(res, memo[i]);
        
        return res;
    }
    
    public int dfs(boolean[][] graph, boolean[] visited, int index){
        if(visited[index]) return memo[index];
        visited[index] = true;
        
        int longest = 1;
        for(int i = 0; i < visited.length; i++){
            if(graph[index][i]){
                longest = Math.max(dfs(graph, visited, i)+1, longest);
            }
        }
        
        memo[index] = longest;
        return longest;
    }
}
```

# Solution 2: DP (Beat 88%)
```
class Solution {
    public int longestStrChain(String[] words) {
        HashMap<String, Integer> map = new HashMap<String, Integer>();
        Arrays.sort(words, (a,b)->(a.length()-b.length()));
        
        int res = 0;
        for(int i = 0; i < words.length; i++){
            int longest = 0;
            for(int j = 0; j < words[i].length(); j++){
                String str = words[i].substring(0, j) + words[i].substring(j+1);
                longest = Math.max(longest, map.getOrDefault(str,0)+1);        
            }
            map.put(words[i], longest);
            res = Math.max(res, longest);
        }
        
        return res;
    }
}
```
