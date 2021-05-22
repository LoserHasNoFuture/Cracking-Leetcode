# 803. Bricks Falling When Hit 305
You are given an  `m x n`  binary  `grid`, where each  `1`  represents a brick and  `0`  represents an empty space. A brick is  **stable**  if:

-   It is directly connected to the top of the grid, or
-   At least one other brick in its four adjacent cells is  **stable**.

You are also given an array  `hits`, which is a sequence of erasures we want to apply. Each time we want to erase the brick at the location  `hits[i] = (rowi, coli)`. The brick on that location (if it exists) will disappear. Some other bricks may no longer be stable because of that erasure and will  **fall**. Once a brick falls, it is  **immediately**  erased from the  `grid`  (i.e., it does not land on other stable bricks).

Return  _an array_ `result`_, where each_ `result[i]` _is the number of bricks that will  **fall**  after the_ `ith` _erasure is applied._

**Note**  that an erasure may refer to a location with no brick, and if it does, no bricks drop.

**Example 1:**

**Input:** grid = [[1,0,0,0],[1,1,1,0]], hits = [[1,0]]
**Output:** [2]
**Explanation:** Starting with the grid:
[[1,0,0,0],
 [1,1,1,0]]
We erase the underlined brick at (1,0), resulting in the grid:
[[1,0,0,0],
 [0,1,1,0]]
The two underlined bricks are no longer stable as they are no longer connected to the top nor adjacent to another stable brick, so they will fall. The resulting grid is:
[[1,0,0,0],
 [0,0,0,0]]
Hence the result is [2].

**Example 2:**

**Input:** grid = [[1,0,0,0],[1,1,0,0]], hits = [[1,1],[1,0]]
**Output:** [0,0]
**Explanation:** Starting with the grid:
[[1,0,0,0],
 [1,1,0,0]]
We erase the underlined brick at (1,1), resulting in the grid:
[[1,0,0,0],
 [1,0,0,0]]
All remaining bricks are still stable, so no bricks fall. The grid remains the same:
[[1,0,0,0],
 [1,0,0,0]]
Next, we erase the underlined brick at (1,0), resulting in the grid:
[[1,0,0,0],
 [0,0,0,0]]
Once again, all remaining bricks are still stable, so no bricks fall.
Hence the result is [0,0].

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `1 <= m, n <= 200`
-   `grid[i][j]`  is  `0`  or  `1`.
-   `1 <= hits.length <= 4 * 104`
-   `hits[i].length == 2`
-   `0 <= xi <= m - 1`
-   `0 <= yi  <= n - 1`
-   All  `(xi, yi)`  are unique.

**Key**
**Remove all hits first, find all unfallen bricks**
**From the last hit bricks, adding each brick back, and count the number of new bricks added**

## Solution 1: Union Find (Beat 38%)
```
class Solution {

    int[][] neighbors = new int[][]{{-1,0},{1,0},{0,1},{0,-1}};
    int m, n;
    int[] id,sz;
    public int[] hitBricks(int[][] grid, int[][] hits) {
        m = grid.length; n = grid[0].length;
        int[] res = new int[hits.length];
        // remove all hitted bricks first
        for(int[] hit: hits) {
            if(grid[hit[0]][hit[1]] == 1) grid[hit[0]][hit[1]] = -1;
        }
        
        id = new int[m*n + 1];
        sz = new int[m*n + 1];
        for(int i = 1; i <= m*n; i++) {
            id[i] = i;
            sz[i] = 1;
        }
        // join all unfallen bricks together
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(grid[i][j] == 1) join_with_neighbors(i,j,grid);
            }
        }
      
	     //adding each brick back, add count the newly added bricks
        int bricks_left = sz[0];
        for(int i = hits.length-1; i >= 0; i--){
            if(grid[hits[i][0]][hits[i][1]] != -1) continue;
            grid[hits[i][0]][hits[i][1]] = 1;
            join_with_neighbors(hits[i][0], hits[i][1],grid);
            res[i] = Math.max(sz[0] - bricks_left - 1, 0);
            bricks_left = sz[0];
        }
        
        return res;
    } 
    
    
    public void join_with_neighbors(int x, int y, int[][] grid){
        if(x == 0) {
            id[x*n+y + 1] = 0;
            sz[0]++;
        }
        
        for(int[] neigh: neighbors){
            int _x = x + neigh[0];
            int _y = y + neigh[1];
            if(_x < 0 || _y < 0 || _x >= m || _y >= n || grid[_x][_y] != 1) continue;
            int r1 = find(x*n+y + 1), r2 = find(_x*n + _y + 1);
            if(r1 == r2) continue;
            if(r1 < r2){
                id[r2] = r1;
                sz[r1] = sz[r1] + sz[r2];
            }else{
                id[r1] = r2;
                sz[r2] = sz[r1] + sz[r2];
            }
        }
        
    }

    public int find(int p){
        while( p != id[p]){
            id[p] = id[id[p]];
            p = id[p];
        }
        return p;
    }
    
}
```

## Solution 2: DFS (Beat 44%)
```
class Solution {
//     DFS, add bricks back
    
    int[][] neighbors = new int[][]{{-1,0},{1,0},{0,1},{0,-1}};
    public int[] hitBricks(int[][] grid, int[][] hits) {
        int[] res = new int[hits.length];
        
//         remove all hits from the grid
        for(int[] hit: hits){
            if(grid[hit[0]][hit[1]] == 1) grid[hit[0]][hit[1]] = -1;
        }
        
//         label all unfallen bricks as 2
        for(int i = 0; i < grid[0].length; i++){
            if(grid[0][i] == 1) {
                grid[0][i] = 2;
                dfs(grid, 0, i);
            }
        }
        
//         adding bricks back
        for(int i = hits.length-1; i >= 0; i--){
            if(grid[hits[i][0]][hits[i][1]] != -1) continue;
//             add bricks back
            grid[hits[i][0]][hits[i][1]] = 1;
//             check if it is connected to top
            if(!connected_to_top(grid,hits[i][0],hits[i][1])) continue;
            
            grid[hits[i][0]][hits[i][1]] = 2;
            res[i] = dfs(grid,hits[i][0],hits[i][1]);
        }
        
        return res;
    }
    
    public boolean connected_to_top(int[][] grid, int x, int y){
        if(x == 0) return true;
        
        for(int[] neigh: neighbors){
            int _x = x + neigh[0];
            int _y = y + neigh[1];
            if(_x < 0 || _y < 0 || _x >= grid.length || _y >= grid[0].length) continue;
            if(grid[_x][_y] == 2) return true;
        }
        
        return false;
    }      
    
    public int dfs(int[][] grid, int x, int y){
        int res = 0;
        
        for(int[] neigh: neighbors){
            int _x = x + neigh[0];
            int _y = y + neigh[1];
            if(_x < 0 || _y < 0 || _x >= grid.length || _y >= grid[0].length 
               || grid[_x][_y] != 1) continue;
            grid[_x][_y] = 2;
            res++;
            res += dfs(grid,_x,_y);
        }
        
        return res;
    }
}
```