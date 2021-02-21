# 694. Number of Distinct Islands
Given a non-empty 2D array  `grid`  of 0's and 1's, an  **island**  is a group of  `1`'s (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Count the number of  **distinct**  islands. An island is considered to be the same as another if and only if one island can be translated (and not rotated or reflected) to equal the other.

**Example 1:**  

11000
11000
00011
00011

Given the above grid map, return `1`.

**Example 2:**  

11011
10000
00001
11011

Given the above grid map, return `3`.  
  
Notice that:

11
1

and

 1
11

are considered different island shapes, because we do not consider reflection / rotation.

**Note:**  The length of each dimension in the given  `grid`  does not exceed 50.

# Solution 1: DFS
Beat 89%
```
class Solution {
    int[][] neighbors = new int[][]{{-1,0},{1,0},{0,-1},{0,1}};
    
    public int numDistinctIslands(int[][] grid) {
        
        HashSet<String> path = new HashSet<String>();
        for(int i = 0; i < grid.length; i++){
            for(int j = 0; j < grid[0].length; j++){
                if(grid[i][j]==1){
                    StringBuilder sb = new StringBuilder();
                    grid[i][j] = 0;
                    dfs(grid,i,j, sb);
                    path.add(sb.toString());
                }
            }
        }
        
        return path.size();
    }

    public void dfs(int[][] grid, int x, int y, StringBuilder sb){
        
        for(int i= 0; i < 4; i++){
            int _x = x + neighbors[i][0];
            int _y = y + neighbors[i][1];
            
            if(_x < 0 || _y < 0 || _x >= grid.length || _y >= grid[0].length || grid[_x][_y] == 0) continue;
            
            if(i == 0) sb.append('l');
            if(i == 1) sb.append('r');
            if(i == 2) sb.append('d');
            if(i == 3) sb.append('u');
            
            grid[_x][_y] = 0;
            dfs(grid,_x,_y,sb);
            sb.append('x');
        }   
    }
}
```

Better coding skill, beat 95%
```
class Solution {
    int[][] neighbors = new int[][]{{-1,0},{1,0},{0,-1},{0,1}};
    
    public int numDistinctIslands(int[][] grid) {
        
        HashSet<String> path = new HashSet<String>();
        for(int i = 0; i < grid.length; i++){
            for(int j = 0; j < grid[0].length; j++){
                if(grid[i][j]==1){
                    StringBuilder sb = new StringBuilder();
                    dfs(grid,i,j, sb,'o');
                    path.add(sb.toString());
                }
            }
        }
        
        return path.size();
    }

    public void dfs(int[][] grid, int x, int y, StringBuilder sb, char next){
        if(x < 0 || y < 0 || x >= grid.length || y >= grid[0].length || grid[x][y] == 0) return;
        
        grid[x][y] = 0;
        sb.append(next);
        dfs(grid,x+1,y,sb,'r');
        dfs(grid,x-1,y,sb,'l');
        dfs(grid,x,y+1,sb,'u');
        dfs(grid,x,y-1,sb,'d');
        sb.append('x');   
    }
}
```