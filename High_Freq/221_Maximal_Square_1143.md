# 221. Maximal Square
Given an  `m x n`  binary  `matrix`  filled with  `0`'s and  `1`'s,  _find the largest square containing only_  `1`'s  _and return its area_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/26/max1grid.jpg)

**Input:** matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
**Output:** 4

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/26/max2grid.jpg)

**Input:** matrix = [["0","1"],["1","0"]]
**Output:** 1

**Example 3:**

**Input:** matrix = [["0"]]
**Output:** 0

**Constraints:**

-   `m == matrix.length`
-   `n == matrix[i].length`
-   `1 <= m, n <= 300`
-   `matrix[i][j]`  is  `'0'`  or  `'1'`.

# Solution 1: Dynamic Programming 
```
class Solution {
    public int maximalSquare(char[][] mat) {
        int max = 0, m = mat.length, n = mat[0].length;
        int[] prev = new int[n];
        int[] dp = new int[n];
        
        for(int i = 0; i < n; i++) {
            dp[i] = mat[0][i] - '0';
            max = Math.max(max,dp[i]);
        }
        
        for(int j = 1; j < m; j++){
            prev = dp;
            dp = new int[n];
            for(int i = 0; i < n; i++){
                if(i == 0) dp[i] = mat[j][i] -'0';
                else if (mat[j][i] == '1'){
                    dp[i] = Math.min(Math.min(prev[i-1],prev[i]), dp[i-1]) + 1;
                }
                max = Math.max(max,dp[i]);
            }
        }
        
        return max*max;
    }
}
```
