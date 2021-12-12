# 2101. Detonate the Maximum Bombs
You are given a list of bombs. The  **range**  of a bomb is defined as the area where its effect can be felt. This area is in the shape of a  **circle**  with the center as the location of the bomb.

The bombs are represented by a  **0-indexed**  2D integer array  `bombs`  where  `bombs[i] = [xi, yi, ri]`.  `xi`  and  `yi`  denote the X-coordinate and Y-coordinate of the location of the  `ith`  bomb, whereas  `ri`  denotes the  **radius**  of its range.

You may choose to detonate a  **single**  bomb. When a bomb is detonated, it will detonate  **all bombs**  that lie in its range. These bombs will further detonate the bombs that lie in their ranges.

Given the list of  `bombs`, return  _the  **maximum**  number of bombs that can be detonated if you are allowed to detonate  **only one**  bomb_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/11/06/desmos-eg-3.png)

**Input:** bombs = [[2,1,3],[6,1,4]]
**Output:** 2
**Explanation:**
The above figure shows the positions and ranges of the 2 bombs.
If we detonate the left bomb, the right bomb will not be affected.
But if we detonate the right bomb, both bombs will be detonated.
So the maximum bombs that can be detonated is max(1, 2) = 2.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/11/06/desmos-eg-2.png)

**Input:** bombs = [[1,1,5],[10,10,5]]
**Output:** 1
**Explanation:** Detonating either bomb will not detonate the other bomb, so the maximum number of bombs that can be detonated is 1.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/11/07/desmos-eg1.png)

**Input:** bombs = [[1,2,3],[2,3,1],[3,4,2],[4,5,3],[5,6,4]]
**Output:** 5
**Explanation:**
The best bomb to detonate is bomb 0 because:
- Bomb 0 detonates bombs 1 and 2. The red circle denotes the range of bomb 0.
- Bomb 2 detonates bomb 3. The blue circle denotes the range of bomb 2.
- Bomb 3 detonates bomb 4. The green circle denotes the range of bomb 3.
Thus all 5 bombs are detonated.

**Constraints:**

-   `1 <= bombs.length <= 100`
-   `bombs[i].length == 3`
-   `1 <= xi, yi, ri  <= 105`

# Solution: DFS
```
class Solution {
    // HashSet<Integer>[] memo;
    int max = 1;
    public int maximumDetonation(int[][] bombs) {
        int n = bombs.length;
        boolean[][] graph = new boolean[n][n];
        build_graph(bombs, graph, n);
       
       // 不能用memo, 因为这个题visited和memo不可兼得>-< 
       // 之后写题一定要仔细考虑这个可能性
        // memo = new HashSet[n];
       
        for(int i = 0; i < n; i++){
            boolean[] visited = new boolean[n];
            max = Math.max(max,dfs(graph, i, visited));
            if(max == n) return max;
        }
        
        return max;
    }
    
    public int dfs(boolean[][] graph, int index, boolean[] visited){
        if(visited[index]) return 0;
        visited[index] = true;
        
        int cur = 1;
        for(int i = 0; i < graph.length; i++){
            if(graph[index][i]) cur += dfs(graph, i, visited);
        }
        
        return cur;
    }
    
    public void build_graph(int[][] bombs, boolean[][] graph, int n){
        
        for(int i = 0; i < n; i++){
            int x = bombs[i][0], y = bombs[i][1], r = bombs[i][2];
            for(int j = 0; j < n; j++){
                if(i == j) graph[i][j] = true;
                else{
                    long distance = (long)(bombs[j][0] - x)*(long)(bombs[j][0] - x) 
                        + (long)(bombs[j][1] - y)*(long)(bombs[j][1] - y);
                    if(distance <= (long)r*(long)r) graph[i][j] = true;
                }
            }
        }
    }
}
```

# Solution 2: BFS
