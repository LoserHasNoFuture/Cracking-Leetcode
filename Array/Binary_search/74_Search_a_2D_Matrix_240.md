# 74. Search a 2D Matrix 240
Write an efficient algorithm that searches for a value in an  _m_  x  _n_  matrix. This matrix has the following properties:

-   Integers in each row are sorted from left to right.
-   The first integer of each row is greater than the last integer of the previous row.

**Example 1:**

**Input:**
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
**Output:** true

**Example 2:**

**Input:**
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
**Output:** false

# Solution: Binary Search
```
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if(matrix.length == 0 ||matrix[0].length == 0|| target < matrix[0][0]) return false;
        int row = matrix.length;
        int col = matrix[0].length;
        
        int start = 0;
        int end = col * row -1;
        
        while(start <= end){
            int mid = (start + end) >>> 1;
            int r = mid/col;
            int c = mid%col;
            if(matrix[r][c] == target) return true;
            if(matrix[r][c] < target) start = mid + 1;
            else end = mid - 1;
        }
        
        return false;
    }
}
```