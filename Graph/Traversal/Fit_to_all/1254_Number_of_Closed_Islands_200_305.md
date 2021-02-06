# 1254. Number of Closed Islands
Given a 2D `grid`  consists of  `0s`  (land) and  `1s`  (water). An  _island_  is a maximal 4-directionally connected group of  `0s`  and a  _closed island_ is an island  **totally** (all left, top, right, bottom) surrounded by  `1s.`

Return the number of  _closed islands_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/10/31/sample_3_1610.png)

**Input:** grid = [[1,1,1,1,1,1,1,0],[1,0,0,0,0,1,1,0],[1,0,1,0,1,1,1,0],[1,0,0,0,0,1,0,1],[1,1,1,1,1,1,1,0]]
**Output:** 2
**Explanation:** 
Islands in gray are closed because they are completely surrounded by water (group of 1s).

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/10/31/sample_4_1610.png)

**Input:** grid = [[0,0,1,0,0],[0,1,0,1,0],[0,1,1,1,0]]
**Output:** 1

**Example 3:**

**Input:** grid = [[1,1,1,1,1,1,1],
               [1,0,0,0,0,0,1],
               [1,0,1,1,1,0,1],
               [1,0,1,0,1,0,1],
               [1,0,1,1,1,0,1],
               [1,0,0,0,0,0,1],
               [1,1,1,1,1,1,1]]
**Output:** 2

**Constraints:**

-   `1 <= grid.length, grid[0].length <= 100`
-   `0 <= grid[i][j] <=1`

# Solution 1: DFS 

#### One way to code (Beat 75%)
```
class Solution {
    int m, n;
    int[][] neighbors = new int[][]{{-1,0},{1,0},{0,-1},{0,1}};
    public int closedIsland(int[][] grid) {
        int cnt = 0;
        m = grid.length;
        n = grid[0].length;
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(grid[i][j] == 0){
                    grid[i][j] = 1;
                    if(dfs(i,j, grid)) cnt++;
                } 
            }
        }
        
        return cnt;
    }
    
    public boolean dfs(int x, int y, int[][] grid){
        boolean closed = true;
        
        for(int[] neigh: neighbors){
            int _x = neigh[0] + x;
            int _y = neigh[1] + y;
            
            if(_x < 0 || _y < 0 || _x >= m || _y >= n) {
                closed = false;
                continue;
            }

            if(grid[_x][_y] == 0) {
                grid[_x][_y] = 1;
                closed = (dfs(_x,_y,grid) && closed);
            }
        }
        
        return closed;
    }
}
``` 

### another way (Beat 100%)
Refer from: [https://leetcode.com/problems/number-of-closed-islands/discuss/688954/JAVA-or-Simple-DFS-solution-or-100-Runtime](https://leetcode.com/problems/number-of-closed-islands/discuss/688954/JAVA-or-Simple-DFS-solution-or-100-Runtime)
```
class Solution {

    boolean flag = true;
    public void dfs(int[][]grid, int i, int j) {

        if( i<0 || j<0 || i>=grid.length || j>=grid[0].length || grid[i][j]==1)
            return;

        //If other 0's are connected to border then dont increase ans
        if((i==0 || j==0 || i==grid.length-1 || j==grid[0].length-1) && grid[i][j]==0)
            flag = false;

        grid[i][j]=1;

        dfs(grid,i-1,j);
        dfs(grid,i+1,j);
        dfs(grid,i,j-1);
        dfs(grid,i,j+1);

    }

    public int closedIsland(int[][] grid) {
        int ans=0;
        for(int i=0;i<grid.length;i++){
            for(int j=0;j<grid[0].length;j++){

                if(grid[i][j]==0)
                {
                    dfs(grid,i,j);

                    //check if 0 isn't border/connected to border
                    if(flag)
                        ans+=1;
                    flag = true;
                }
            }
        }
        return ans;
    }
}
```