# 1079. Letter Tile Possibilities (Not Finished)
You have  `n` `tiles`, where each tile has one letter  `tiles[i]`  printed on it.

Return  _the number of possible non-empty sequences of letters_  you can make using the letters printed on those  `tiles`.

**Example 1:**

**Input:** tiles = "AAB"
**Output:** 8
**Explanation:** The possible sequences are "A", "B", "AA", "AB", "BA", "AAB", "ABA", "BAA".

**Example 2:**

**Input:** tiles = "AAABBC"
**Output:** 188

**Example 3:**

**Input:** tiles = "V"
**Output:** 1

**Constraints:**

-   `1 <= tiles.length <= 7`
-   `tiles`  consists of uppercase English letters.

# Solution 1: BackTracking 
Note that the relative order or characters would change.
```
class Solution {
    // (Beat 20%)
    public int numTilePossibilities(String tiles) {
        if(tiles.length() == 1) return 1;
        
        HashSet<String> set = new HashSet<String>();
        StringBuilder sb = new StringBuilder();
        boolean[] visited = new boolean[tiles.length()];
        
        dfs(tiles,-1,set,sb,visited);    
        
        return set.size();
    }
    
    public void dfs(String tiles, int index, HashSet<String> set, StringBuilder sb, boolean[] visited){
        
        if(index >= 0){
            sb.append(tiles.charAt(index));
            set.add(sb.toString());            
        }
        
        for(int i = 0; i < tiles.length(); i++){
            if(!visited[i]){
                visited[i] = true;
                dfs(tiles,i,set,sb,visited);
                visited[i] = false;
            }
        }
        
        if(index >= 0) sb.delete(sb.length()-1,sb.length());    
    }
}
```

Improved:
```
class Solution {
    // (Beat 44%)
    int res = 0;
    public int numTilePossibilities(String tiles) {
        if(tiles.length() == 1) return 1;
        
        StringBuilder sb = new StringBuilder();
        boolean[] visited = new boolean[tiles.length()];
        
        dfs(tiles,-1,sb,visited);    
        
        return res;
    }
    
    public void dfs(String tiles, int index, StringBuilder sb, boolean[] visited){
        
        if(index >= 0){
            sb.append(tiles.charAt(index));
            res++;
        }
        
        HashSet<Character> set = new HashSet<Character>();
        for(int i = 0; i < tiles.length(); i++){
            if(!visited[i] && !set.contains(tiles.charAt(i))){
                visited[i] = true;
                set.add(tiles.charAt(i));
                dfs(tiles,i,sb,visited);
                visited[i] = false;
            }
        }
        
        if(index >= 0) sb.delete(sb.length()-1,sb.length());    
    }
}
```