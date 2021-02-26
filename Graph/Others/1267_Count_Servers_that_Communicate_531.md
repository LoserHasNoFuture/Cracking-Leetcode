# 1267. Count Servers that Communicate
You are given a map of a server center, represented as a  `m * n`  integer matrix `grid`, where 1 means that on that cell there is a server and 0 means that it is no server. Two servers are said to communicate if they are on the same row or on the same column.  
  
Return the number of servers that communicate with any other server.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/11/14/untitled-diagram-6.jpg)

**Input:** grid = [[1,0],[0,1]]
**Output:** 0
**Explanation:** No servers can communicate with others.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2019/11/13/untitled-diagram-4.jpg)**

**Input:** grid = [[1,0],[1,1]]
**Output:** 3
**Explanation:** All three servers can communicate with at least one other server.

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/11/14/untitled-diagram-1-3.jpg)

**Input:** grid = [[1,1,0,0],[0,0,1,0],[0,0,1,0],[0,0,0,1]]
**Output:** 4
**Explanation:** The two servers in the first row can communicate with each other. The two servers in the third column can communicate with each other. The server at right bottom corner can't communicate with any other server.

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `1 <= m <= 250`
-   `1 <= n <= 250`
-   `grid[i][j] == 0 or 1`

# Solution 1: DFS (Beat 14%)
```
class Solution {
        
    public int countServers(int[][] grid) {
        int cnt = 0;
        
        for(int i = 0; i < grid.length; i++){
            for(int j = 0;j < grid[0].length; j++){
                if(grid[i][j] == 0) grid[i][j] = 2;
                if(grid[i][j] == 1) {
                    grid[i][j] = 2;
                    int connected = dfs(grid,i,j);
                    if(connected > 0) cnt += (1 + connected);
                }
            }
        }
        
        return cnt;
    }
    
    public int dfs(int[][] grid, int x, int y){
        int cnt = 0;
        for(int i = 0; i < grid.length; i++){
            if(grid[i][y] == 1){
                cnt++;
                grid[i][y] = 2;
                cnt += dfs(grid,i,y);
            }
        }
        
        for(int i = 0; i < grid[0].length; i++){
            if(grid[x][i] == 1){
                cnt++;
                grid[x][i] = 2;
                cnt += dfs(grid,x,i);
            }
        }
        
        return cnt;
    }
}
```

# Solution 2: Count For Rows and Columns (Beat 100%)
Refer from: [https://leetcode.com/problems/count-servers-that-communicate/discuss/436188/Java-or-Clean-And-Simple-or-Beats-100](https://leetcode.com/problems/count-servers-that-communicate/discuss/436188/Java-or-Clean-And-Simple-or-Beats-100)
```
class Solution {
    
    public int countServers(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) return 0;
        int numRows = grid.length;
        int numCols = grid[0].length;
        int rowCount[] = new int[numRows];
        int colCount[] = new int[numCols];
        int totalServers = 0;
        for (int row = 0; row < numRows; row++) {
            for (int col = 0; col < numCols; col++) {
                if (grid[row][col] == 1) {
                    rowCount[row]++;
                    colCount[col]++;
                    totalServers++;
                }
            }
        }
        for (int row = 0; row < numRows; row++) {
            for (int col = 0; col < numCols; col++) {
                if (grid[row][col] == 1) {
                    if (rowCount[row] == 1 && colCount[col] == 1) {
                        totalServers--;
                    }
                }
            }
        }
        return totalServers;
    }
}
```
