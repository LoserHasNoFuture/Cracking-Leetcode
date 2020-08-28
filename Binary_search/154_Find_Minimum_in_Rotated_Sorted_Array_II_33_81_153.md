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
        int res = Integer.MAX_VALUE;
        
        while(start < end){
            int mid = (start + end) >>> 1;
            if(nums[start] == nums[end]){
                res = Math.min(nums[start],res);
                start++;end--;
                continue;
            }
            
            if(nums[start] <= nums[mid] && nums[mid] <= nums[end]){
//                 in order
                end = mid - 1;
            }else if (nums[start] <= nums[mid] && nums[mid] >= nums[end]){
//                 pivot in right
                start = mid + 1;
            }else{
//                 pivot in left
                if(mid == 1) return Math.min(nums[mid],res);
                int count = 1;
                while(mid - count > start && nums[mid] == nums[mid - count]) count++;
                if(mid - count > start && nums[mid] > nums[mid - count]) end = mid - count;
                else return Math.min(nums[mid],res);
                
            }
        }
        
        return Math.min(nums[start],res);
    }
}
```