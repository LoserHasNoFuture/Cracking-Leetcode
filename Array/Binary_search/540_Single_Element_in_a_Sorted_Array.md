# 540. Single Element in a Sorted Array
You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once.

Return  _the single element that appears only once_.

Your solution must run in  `O(log n)`  time and  `O(1)`  space.

**Example 1:**

**Input:** nums = [1,1,2,3,3,4,4,8,8]
**Output:** 2

**Example 2:**

**Input:** nums = [3,3,7,7,10,11,11]
**Output:** 10

**Constraints:**

-   `1 <= nums.length <= 105`
-   `0 <= nums[i] <= 105`

# Solution: Binary Search
```
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int n = nums.length, start = 0, end = n-1;
        
        while(start < end){
            int mid = (start + end) >>> 1;
            if(mid%2 == 0){
                if(nums[mid+1] != nums[mid]) end = mid;
                else start = mid+1;
            } else{
                if(nums[mid-1] != nums[mid]) end = mid;
                else start = mid + 1;
            }
        }
        
        return nums[start];
    }
}
```
