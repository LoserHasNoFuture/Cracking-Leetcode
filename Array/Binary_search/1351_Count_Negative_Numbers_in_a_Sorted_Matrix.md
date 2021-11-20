# 1351. Count Negative Numbers in a Sorted Matrix

Given a  `m * n` matrix  `grid` which is sorted in non-increasing order both row-wise and column-wise.

Return the number of  **negative**  numbers in `grid`.

**Example 1:**

**Input:** grid = [[4,3,2,-1],[3,2,1,-1],[1,1,-1,-2],[-1,-1,-2,-3]]
**Output:** 8
**Explanation:** There are 8 negatives number in the matrix.

**Example 2:**

**Input:** grid = [[3,2],[1,0]]
**Output:** 0

**Example 3:**

**Input:** grid = [[1,-1],[-1,-1]]
**Output:** 3

**Example 4:**

**Input:** grid = [[-1]]
**Output:** 1

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `1 <= m, n <= 100`
-   `-100 <= grid[i][j] <= 100`

# Solution 1
Refer from: [https://leetcode.com/problems/count-negative-numbers-in-a-sorted-matrix/discuss/510249/JavaPython-3-2-similar-O(m-%2B-n)-codes-w-brief-explanation-and-analysis.](https://leetcode.com/problems/count-negative-numbers-in-a-sorted-matrix/discuss/510249/JavaPython-3-2-similar-O(m-%2B-n)-codes-w-brief-explanation-and-analysis.)


This solution uses the fact that the negative regions of the matrix will form a "staircase" shape, e.g.:
```
++++++
++++--
++++--
+++---
+-----
+-----
```
What this solution then does is to "trace" the outline of the staircase.
```
class Solution {
// Start from bottom-left corner of the matrix
// count the negative numbers in each row.
    public int countNegatives(int[][] grid) {
        int m = grid.length, n = grid[0].length, r = m - 1, c = 0, cnt = 0;
        while (r >= 0 && c < n) {
            if (grid[r][c] < 0) {
                --r;
                cnt += n - c; // there are n - c negative numbers in current row.
            }else {
                ++c;
            }
        }
        return cnt;
    }
}
```

# Solution 2: Binary Search

```
class Solution {
    int count = 0;
    public int countNegatives(int[][] grid) {
        if(grid.length == 0) return count;
        
        int start = 0;
        for(int i = grid.length - 1; i >= 0; i--){
            start = find_next_start_point(start, grid[i]);
        }
        
        return count;
    }
    
    public int find_next_start_point(int start, int[] row){
        int end = row.length - 1;
        
        if(row[end] > 0) return end;
        
        if(row[start] < 0) {
            count += end - start + 1;
            return start;
        }
        
        int neg_start = 0;
        
        // binary search
        while(start <= end){
            int mid = (end + start) / 2;
            if(row[mid] < 0) end = mid - 1;
            else start = mid + 1;
        }
        
        if(neg_start == 0) neg_start = row[end] < 0 ? end:start;

        count += row.length - neg_start;
        
        return neg_start;
    }
}
```