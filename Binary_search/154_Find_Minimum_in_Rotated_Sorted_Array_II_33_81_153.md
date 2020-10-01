# 154. Find Minimum in Rotated Sorted Array II 33 81 153
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).

Find the minimum element.

The array may contain duplicates.

**Example 1:**

**Input:** [1,3,5]
**Output:** 1

**Example 2:**

**Input:** [2,2,2,0,1]
**Output:** 0

**Note:**

-   This is a follow up problem to [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/).
-   Would allow duplicates affect the run-time complexity? How and why?

# Solution: Binary Search
```
class Solution {
    public int findMin(int[] nums) {
        int start = 0;
        int end = nums.length - 1;
        
        while(start < end){
            if(nums[start] == nums[end]){
                start++;
                continue;
            }
            int mid = (start + end) >>> 1;
//             mid must be greater than end
            if(nums[mid] > nums[end]){
                start = mid + 1;
//             mid must be smaller than start
            }else if(nums[mid] < nums[start]){
                end = mid;
            }else{
                end = start;
            }
        }
        
        return nums[start];
    }
}
```
Clean code:
```
class Solution {
    public int findMin(int[] nums) {
        int start = 0;
        int end = nums.length - 1;
        while(start < end){
            int mid = (start + end) >>> 1;
            if(nums[mid] > nums[end]) start = mid + 1;
            else if(nums[mid] == nums[end]) end--;
            else end = mid;
        }
        return nums[start];
    }
}
```


