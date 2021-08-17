# 73. Set Matrix Zeroes
Given an  `m x n`  integer matrix  `matrix`, if an element is  `0`, set its entire row and column to  `0`'s, and return  _the matrix_.

You must do it  [in place](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg)

**Input:** matrix = [[1,1,1],[1,0,1],[1,1,1]]
**Output:** [[1,0,1],[0,0,0],[1,0,1]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg)

**Input:** matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
**Output:** [[0,0,0,0],[0,4,5,0],[0,3,1,0]]

**Constraints:**

-   `m == matrix.length`
-   `n == matrix[0].length`
-   `1 <= m, n <= 200`
-   `-231  <= matrix[i][j] <= 231  - 1`

**Follow up:**

-   A straightforward solution using  `O(mn)`  space is probably a bad idea.
-   A simple improvement uses  `O(m + n)`  space, but still not the best solution.
-   Could you devise a constant space solution?


# Solution: O(1) Space (Beat 100%)
They are using the first row and column as a memory to keep track of all the 0's in the entire matrix.
```
class Solution {
    public void setZeroes(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        boolean frow = false, fcol = false;
        
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(mat[i][j] == 0){
                    if(i == 0) frow = true;
                    if(j == 0) fcol = true;
                    mat[i][0] = 0;
                    mat[0][j] = 0;
                }
            }
        }
        
        for(int i = 1; i < m; i++){
            if(mat[i][0] == 0){
                for(int j = 0; j < n; j++) mat[i][j] = 0;
            }
        }
        
        for(int j = 1; j < n; j++){
            if(mat[0][j] == 0){
                for(int i = 0; i < m; i++) mat[i][j] = 0;
            }
        }
        
        if(frow){
            for(int j = 0; j < n; j++) mat[0][j] = 0;
        }
        
        if(fcol){
            for(int i = 0; i < m; i++) mat[i][0] = 0;
        }
    }
}
```