# 1428. Leftmost Column with at Least a One
_(This problem is an  **interactive problem**.)_

A  **row-sorted binary matrix**  means that all elements are  `0`  or  `1`  and each row of the matrix is sorted in non-decreasing order.

Given a  **row-sorted binary matrix**  `binaryMatrix`, return  _the index (0-indexed) of the  **leftmost column**  with a 1 in it_. If such an index does not exist, return  `-1`.

**You can't access the Binary Matrix directly.**  You may only access the matrix using a  `BinaryMatrix`  interface:

-   `BinaryMatrix.get(row, col)`  returns the element of the matrix at index  `(row, col)`  (0-indexed).
-   `BinaryMatrix.dimensions()`  returns the dimensions of the matrix as a list of 2 elements  `[rows, cols]`, which means the matrix is  `rows x cols`.

Submissions making more than  `1000`  calls to  `BinaryMatrix.get`  will be judged  _Wrong Answer_. Also, any solutions that attempt to circumvent the judge will result in disqualification.

For custom testing purposes, the input will be the entire binary matrix  `mat`. You will not have access to the binary matrix directly.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2019/10/25/untitled-diagram-5.jpg)**

**Input:** mat = [[0,0],[1,1]]
**Output:** 0

**Example 2:**

**![](https://assets.leetcode.com/uploads/2019/10/25/untitled-diagram-4.jpg)**

**Input:** mat = [[0,0],[0,1]]
**Output:** 1

**Example 3:**

**![](https://assets.leetcode.com/uploads/2019/10/25/untitled-diagram-3.jpg)**

**Input:** mat = [[0,0],[0,0]]
**Output:** -1

**Example 4:**

**![](https://assets.leetcode.com/uploads/2019/10/25/untitled-diagram-6.jpg)**

**Input:** mat = [[0,0,0,1],[0,0,1,1],[0,1,1,1]]
**Output:** 1

**Constraints:**

-   `rows == mat.length`
-   `cols == mat[i].length`
-   `1 <= rows, cols <= 100`
-   `mat[i][j]`  is either  `0`  or  `1`.
-   `mat[i]`  is sorted in non-decreasing order.

# Solution 1: Binary Search by Row O(nlogm), Beat 100%
```
class Solution {
    public int leftMostColumnWithOne(BinaryMatrix mat) {
        List dim = mat.dimensions();
        int rows = (int)dim.get(0), cols = (int)dim.get(1);
        
        int min_col = cols;
        for(int i = 0; i < rows; i++){
            int index = find_index(mat, i, min_col);
            min_col = Math.min(min_col,index);
        }
        
        if(min_col == cols) min_col = -1;
        return min_col;
    }
    
//     left most
    public int find_index(BinaryMatrix mat, int row, int min_col){
        int start = 0, end = min_col;
        while(start < end){
            int mid = (start + end) >>> 1;
            if(mat.get(row,mid) >= 1) end = mid;
            else start = mid + 1;
        }
        return start;
    } 
}
```

# Solution 2: Binary Search by Column, O(mlogn)
Refer From: [https://leetcode.com/problems/leftmost-column-with-at-least-a-one/discuss/590828/Java-Binary-Search-and-Linear-Solutions-with-Picture-Explain-Clean-Code](https://leetcode.com/problems/leftmost-column-with-at-least-a-one/discuss/590828/Java-Binary-Search-and-Linear-Solutions-with-Picture-Explain-Clean-Code)
```
class Solution {
    public int leftMostColumnWithOne(BinaryMatrix binaryMatrix) {
        List<Integer> dimen = binaryMatrix.dimensions();
        int m = dimen.get(0), n = dimen.get(1);
        int left = 0, right = n - 1, ans = -1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (existOneInColumn(binaryMatrix, m, mid)) {
                ans = mid;          // record as current ans
                right = mid - 1;    // try to find in the left side
            } else {
                left = mid + 1;     // try to find in the right side
            }
        }
        return ans;
    }
    boolean existOneInColumn(BinaryMatrix binaryMatrix, int m, int c) {
        for (int r = 0; r < m; r++) if (binaryMatrix.get(r, c) == 1) return true;
        return false;
    }
}
```

# Solution 3: Linear Solution, O(m + n), Beat 100%
```
class Solution {
    public int leftMostColumnWithOne(BinaryMatrix mat) {
        List dim = mat.dimensions();
        int rows = (int)dim.get(0), cols = (int)dim.get(1);
        
        int res = cols, col = cols - 1, row = 0;
        while(row < rows && col >= 0){
            if(mat.get(row,col) == 1){
                res = col;
                col--;
            }else{
                row++;
            }
        }
        
        return res == cols? -1 : res;
    }   
}
```