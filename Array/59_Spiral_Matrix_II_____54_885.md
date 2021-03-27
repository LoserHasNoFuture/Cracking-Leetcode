# 59. Spiral Matrix II 54 885
Given a positive integer  `n`, generate an  `n x n`  `matrix`  filled with elements from  `1`  to  `n2`  in spiral order.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiraln.jpg)

**Input:** n = 3
**Output:** [[1,2,3],[8,9,4],[7,6,5]]

**Example 2:**

**Input:** n = 1
**Output:** [[1]]

**Constraints:**

-   `1 <= n <= 20`

# Solution: Straight Foward (Beat 100%)
```
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] mat = new int[n][n];
        int max = n*n;
        
        int left = 0, right = n-1, top = 0, bottom = n-1;
        int count = 1;
        while(true){
            
            if(count > max) break;
            for(int i = left; i <= right && count <= max; i++) mat[top][i] = count++;
            top++;
            
            for(int i = top; i <= bottom && count <= max; i++) mat[i][right] = count++;
            right--;
            
            for(int i = right; i >= left && count <= max; i--) mat[bottom][i] = count++;
            bottom--;
            
            for(int i = bottom; i >= top && count <= max; i--) mat[i][left] = count++;
            left++;
        }
        
        return mat;
    }
}
```