# 980. Unique Paths III

On a 2-dimensional `grid`, there are 4 types of squares:

-   `1`  represents the starting square. There is exactly one starting square.
-   `2`  represents the ending square. There is exactly one ending square.
-   `0`  represents empty squares we can walk over.
-   `-1`  represents obstacles that we cannot walk over.

Return the number of 4-directional walks from the starting square to the ending square, that  **walk over every non-obstacle square exactly once**.

**Example 1:**

**Input:** [[1,0,0,0],[0,0,0,0],[0,0,2,-1]]
**Output:** 2
**Explanation:** We have the following two paths: 
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2)
2. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2)

**Example 2:**

**Input:** [[1,0,0,0],[0,0,0,0],[0,0,0,2]]
**Output:** 4
**Explanation:** We have the following four paths: 
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2),(2,3)
2. (0,0),(0,1),(1,1),(1,0),(2,0),(2,1),(2,2),(1,2),(0,2),(0,3),(1,3),(2,3)
3. (0,0),(1,0),(2,0),(2,1),(2,2),(1,2),(1,1),(0,1),(0,2),(0,3),(1,3),(2,3)
4. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2),(2,3)

**Example 3:**

**Input:** [[0,1],[2,0]]
**Output:** 0
**Explanation:** 
There is no path that walks over every empty square exactly once.
Note that the starting and ending square can be anywhere in the grid.

**Note:**

1.  `1 <= grid.length * grid[0].length <= 20`

# Solution 1: BackTracking  (Beat 100%)
```
class Solution {
    int res = 0, empty = 0;
    int[][] neighbors = {{-1,0},{1,0},{0,-1},{0,1}};
    int row, col;
    int obstacles = 0;
    public int uniquePathsIII(int[][] grid) {
        row = grid.length;
        col = grid[0].length;
        int sx = 0,sy = 0;
        for(int i = 0; i < row; i++){
            for(int j = 0; j < col; j++){
                if(grid[i][j] == 0) empty++;
                if(grid[i][j] == 1){
                    sx = i; sy=j;
                    grid[i][j] = -1;
                }
            }
        }
        
        dfs(grid, sx, sy);  
        return res;
    }
    
    
    public void dfs(int[][] grid, int x, int y){
    
        for(int[] neigh:neighbors){
            int next_x = x + neigh[0];
            int next_y = y + neigh[1];
            
            if(next_x < 0 || next_y < 0 ||
              next_x >= row || next_y >= col ||
              grid[next_x][next_y] == -1) continue;
            
            if(grid[next_x][next_y] == 2) {
                if(empty == 0) res++;
                continue;
            }
            
            empty--;
            grid[next_x][next_y] = -1;
            dfs(grid,next_x,next_y);
            grid[next_x][next_y] = 0;
            empty++;
        }    
        
    }
    
}
```