# 329. Longest Increasing Path in a Matrix 1340
Given an  `m x n`  integers  `matrix`, return  _the length of the longest increasing path in_ `matrix`.

From each cell, you can either move in four directions: left, right, up, or down. You  **may not**  move  **diagonally**  or move  **outside the boundary**  (i.e., wrap-around is not allowed).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/05/grid1.jpg)

**Input:** matrix = [[9,9,4],[6,6,8],[2,1,1]]
**Output:** 4
**Explanation:** The longest increasing path is `[1, 2, 6, 9]`.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/27/tmp-grid.jpg)

**Input:** matrix = [[3,4,5],[3,2,6],[2,2,1]]
**Output:** 4
**Explanation:** The longest increasing path is `[3, 4, 5, 6]`. Moving diagonally is not allowed.

**Example 3:**

**Input:** matrix = [[1]]
**Output:** 1

**Constraints:**

-   `m == matrix.length`
-   `n == matrix[i].length`
-   `1 <= m, n <= 200`
-   `0 <= matrix[i][j] <= 231  - 1`


# Solution 1: DFS + Memo, O(mn)
Since it is strictly increasing path, so visited matrix is not necessary.
```
class Solution {
    int[][] memo;
    int[][] neighbors = new int[][] {{-1,0},{1,0},{0,1},{0,-1}};
    int m,n, res = 0;
    public int longestIncreasingPath(int[][] mat) {
        m = mat.length; 
        n = mat[0].length;
        memo = new int[m][n];
        
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                dfs(mat,i,j);
                res = Math.max(res,memo[i][j]);
            }
        }
        
        return res;
    }
    
    public int dfs(int[][] mat, int x, int y){
        if(memo[x][y] != 0) return memo[x][y];
        int cnt = 1;
        
        for(int[] nei: neighbors){
            int _x = x + nei[0];
            int _y = y + nei[1];
            if(_x < 0 || _y < 0 || _x >= m || _y >= n || mat[_x][_y] <= mat[x][y])  continue;
            cnt = Math.max(cnt, dfs(mat,_x,_y) + 1);
        }
        
        memo[x][y] = cnt;
        return cnt;
    }
}
```

# Solution 2: Topology Sort, O(mn)
Refer from: [https://leetcode.com/problems/longest-increasing-path-in-a-matrix/discuss/288520/Longest-Path-in-DAG](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/discuss/288520/Longest-Path-in-DAG)
```
class Solution {
    int[][] DIRS = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    public int longestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return 0;
        }
        int m = matrix.length;
        int n = matrix[0].length;
        int[][] grid = new int[m + 2][n + 2];
        // 把越界的都设为Integer.MAX_VALUE;
        for (int i = 0; i <= m + 1; i++) {
           for (int j = 0; j <= n + 1; j++) {
               if (i == 0 || i == m + 1 || j == 0 || j == n + 1) {
                   grid[i][j] = Integer.MAX_VALUE;
               } else {
                   grid[i][j] =  matrix[i - 1][j - 1];
               }
           }
        }
        // 计算indegree
        int[][] indgrees = new int[m + 2][n + 2];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                for (int[] dir : DIRS) {
                    if (grid[i][j] > grid[i + dir[0]][j + dir[1]]) {
                        indgrees[i][j]++;
                    }
                }
            }
        }
        
        Queue<int[]> leaves = new LinkedList<>();
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (indgrees[i][j] == 0) {
                    leaves.add(new int[]{i, j});
                }
            }
        }
        
        int level = 0;
        while (!leaves.isEmpty()) {
            level++;
            int l = leaves.size();
            for (int i = 0; i < l; i++) {
                int[] leaf = leaves.poll();
                for (int[] dir : DIRS) {
                    int nextX = dir[0] + leaf[0];
                    int nextY = dir[1] + leaf[1];
                    if (grid[nextX][nextY] > grid[leaf[0]][leaf[1]]) {
                        if (--indgrees[nextX][nextY] == 0) {
                            leaves.add(new int[]{nextX, nextY});
                        }
                    }
                }
            }
        }
        return level;
    }
}
```