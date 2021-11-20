# 1198. Find Smallest Common Element in All Rows
Given an  `m x n`  matrix  `mat`  where every row is sorted in  **strictly**  **increasing**  order, return  _the  **smallest common element**  in all rows_.

If there is no common element, return  `-1`.

**Example 1:**

**Input:** mat = [[1,2,3,4,5],[2,4,5,8,10],[3,5,7,9,11],[1,3,5,7,9]]
**Output:** 5

**Example 2:**

**Input:** mat = [[1,2,3],[2,3,4],[2,3,5]]
**Output:** 2

**Constraints:**

-   `m == mat.length`
-   `n == mat[i].length`
-   `1 <= m, n <= 500`
-   `1 <= mat[i][j] <= 104`
-   `mat[i]`  is sorted in strictly increasing order.

# Solution 1: HashMap, Count O(mn) time, O(mn) space
```
class Solution {
    public int smallestCommonElement(int[][] mat) {
        HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
        
        for(int i = 0; i < mat.length; i++){
            for(int num: mat[i]){
                map.put(num, map.getOrDefault(num,0)+1);
                if(i == mat.length - 1 && map.get(num) == mat.length) return num;
            }
        }
        
        return -1;
    }
}
``` 

# Solution 2: Binary Search O(mnlogn) Time, O(1) Space
```
class Solution {
    public int smallestCommonElement(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        if(m == 1) return mat[0][0];
        int[] start = new int[m], end = new int[m];
        Arrays.fill(end, n);
        
        for(int target: mat[0]){
            boolean flag = true;
            for(int i = 1; i < m; i++){
                start[i] = binary_search(mat[i],target,start[i],end[i]);
                if(start[i] == n) return -1;
                if(mat[i][start[i]] != target) flag = false;
            }
            if(flag) return target;
        }
        
        return -1;
    }
    
//     greater than or equal to
    public int binary_search(int[] nums, int target, int start, int end){
        while(start < end){
            int mid = (start + end) >>> 1;
            if(nums[mid] == target) return mid;
            if(nums[mid] < target) start = mid + 1;
            else end = mid;
        }
        return start;
    }
}
```