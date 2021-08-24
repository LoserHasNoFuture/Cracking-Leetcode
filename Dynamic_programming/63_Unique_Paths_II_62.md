# 63. Unique Paths II
A robot is located at the top-left corner of a  `m x n`  grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and space is marked as  `1`  and  `0`  respectively in the grid.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/04/robot1.jpg)

**Input:** obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
**Output:** 2
**Explanation:** There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/04/robot2.jpg)

**Input:** obstacleGrid = [[0,1],[0,0]]
**Output:** 1

**Constraints:**

-   `m == obstacleGrid.length`
-   `n == obstacleGrid[i].length`
-   `1 <= m, n <= 100`
-   `obstacleGrid[i][j]`  is  `0`  or  `1`.

# Solution: Dynamic Programming 
```
class Solution {
    public int uniquePathsWithObstacles(int[][] map) {
        int m = map[0].length, n = map.length;
        int[] dp = new int[m];
        for(int i = 0; i < m; i++) {
        /*  !!(i == 0 || dp[i-1] == 1)!! */
            if(map[0][i] != 1 && (i == 0 || dp[i-1] == 1)) dp[i] = 1;
        }
        
        
        for(int j = 1; j < n; j++){
            if(map[j][0] == 1) dp[0] = 0;
            for(int i = 1; i < m; i++){
                if(map[j][i] == 1) dp[i] = 0;
                else dp[i] = dp[i] + dp[i-1];
            }
        }
        
        return dp[m-1];
    }
}
```