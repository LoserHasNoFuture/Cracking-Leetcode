# 994. Rotting Oranges 286
In a given grid, each cell can have one of three values:

-   the value  `0`  representing an empty cell;
-   the value  `1`  representing a fresh orange;
-   the value  `2`  representing a rotten orange.

Every minute, any fresh orange that is adjacent (4-directionally) to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return  `-1`  instead.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2019/02/16/oranges.png)**

**Input:** [[2,1,1],[1,1,0],[0,1,1]]
**Output:** 4

**Example 2:**

**Input:** [[2,1,1],[0,1,1],[1,0,1]]
**Output:** -1
**Explanation: ** The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.

**Example 3:**

**Input:** [[0,2]]
**Output:** 0
**Explanation: ** Since there are already no fresh oranges at minute 0, the answer is just 0.

**Note:**

1.  `1 <= grid.length <= 10`
2.  `1 <= grid[0].length <= 10`
3.  `grid[i][j]`  is only  `0`,  `1`, or  `2`.

# Solution 1: BFS
```
class Solution {
    public int orangesRotting(int[][] grid) {
        int res = 0;
        Queue<int[]> queue = new LinkedList<int[]>();
        int[][] neighbors = new int[][]{{-1,0},{1,0},{0,1},{0,-1}};
        
        
        for(int i = 0; i < grid.length; i++)
            for(int j = 0; j < grid[0].length; j++)
                if(grid[i][j] == 2) queue.offer(new int[]{i,j});
        
        while(!queue.isEmpty()){
            int sz = queue.size();
            for(int i = 0; i < sz; i++){
                int[] index = queue.poll();
                
                for(int[] neigh : neighbors){
                    int next_r = index[0] + neigh[0];
                    int next_c = index[1] + neigh[1];
                    if(next_r >= 0 && next_r < grid.length
                      && next_c >= 0 && next_c < grid[0].length
                      && grid[next_r][next_c] == 1){
                        grid[next_r][next_c] = 2;
                        queue.offer(new int[]{next_r,next_c});
                    }
                }
            }
            res++;
        }
        
        for(int i = 0; i < grid.length; i++)
            for(int j = 0; j < grid[0].length; j++)
                if(grid[i][j] == 1) return -1;

        return res-1 > 0 ? res - 1 : 0;
    }
}
```