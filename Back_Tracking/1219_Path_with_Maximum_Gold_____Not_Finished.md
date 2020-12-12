# 1219. Path with Maximum Gold  (Not Finished)
In a gold mine  `grid` of size  `m * n`, each cell in this mine has an integer representing the amount of gold in that cell, `0`  if it is empty.

Return the maximum amount of gold you can collect under the conditions:

-   Every time you are located in a cell you will collect all the gold in that cell.
-   From your position you can walk one step to the left, right, up or down.
-   You can't visit the same cell more than once.
-   Never visit a cell with `0`  gold.
-   You can start and stop collecting gold from **any** position in the grid that has some gold.

**Example 1:**

**Input:** grid = [[0,6,0],[5,8,7],[0,9,0]]
**Output:** 24
**Explanation:**
[[0,6,0],
 [5,8,7],
 [0,9,0]]
Path to get the maximum gold, 9 -> 8 -> 7.

**Example 2:**

**Input:** grid = [[1,0,7],[2,0,6],[3,4,5],[0,3,0],[9,0,20]]
**Output:** 28
**Explanation:**
[[1,0,7],
 [2,0,6],
 [3,4,5],
 [0,3,0],
 [9,0,20]]
Path to get the maximum gold, 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7.

**Constraints:**

-   `1 <= grid.length, grid[i].length <= 15`
-   `0 <= grid[i][j] <= 100`
-   There are at most  **25** cells containing gold.

# Solution 1: BackTracking (Beat 48%)
```
class Solution {
    
    int max = 0;
    int[][] neighbors = {{-1,0},{1,0},{0,1},{0,-1}};
    public int getMaximumGold(int[][] grid) {
        int row = grid.length;
        int col = grid[0].length;
        boolean[][] visited = new boolean[row][col];
            
        for(int i = 0; i < grid.length; i++){
            for(int j = 0; j < grid[0].length; j++){
                visited[i][j] = true;
                dfs(grid, visited, i, j, 0);        
                visited[i][j] = false;
            }
        }
        
        return max;
    }
    
    public void dfs(int[][] grid, boolean[][] visited, int m, int n, int sum){
        if(grid[m][n] == 0) return;
        sum += grid[m][n];
        
        boolean has_next = false;
        
        for(int[] neigh: neighbors){
            int next_x = m+neigh[0];
            int next_y = n+neigh[1];
            if(next_x < 0 || next_y < 0 || 
               next_x >= grid.length || next_y >= grid[0].length ||
              grid[next_x][next_y] == 0 || visited[next_x][next_y]) continue;
            has_next = true;
            visited[next_x][next_y] = true;
            dfs(grid,visited,next_x,next_y, sum);
            visited[next_x][next_y] = false;
        }
        
        if(!has_next && sum > max) max = sum;
        
    }
}
```