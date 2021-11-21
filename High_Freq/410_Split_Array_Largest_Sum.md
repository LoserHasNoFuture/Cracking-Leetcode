# 410. Split Array Largest Sum
Given an array  `nums`  which consists of non-negative integers and an integer  `m`, you can split the array into  `m`  non-empty continuous subarrays.

Write an algorithm to minimize the largest sum among these  `m`  subarrays.

**Example 1:**

**Input:** nums = [7,2,5,10,8], m = 2
**Output:** 18
**Explanation:**
There are four ways to split nums into two subarrays.
The best way is to split it into [7,2,5] and [10,8],
where the largest sum among the two subarrays is only 18.

**Example 2:**

**Input:** nums = [1,2,3,4,5], m = 2
**Output:** 9

**Example 3:**

**Input:** nums = [1,4,4], m = 3
**Output:** 4

**Constraints:**

-   `1 <= nums.length <= 1000`
-   `0 <= nums[i] <= 106`
-   `1 <= m <= min(50, nums.length)`

# Solution: Binary Search
```
class Solution {
    public int splitArray(int[] nums, int m) {
        int sum = 0, max = 0;
        for(int num: nums){
            sum += num;
            if(num > max) max = num;
        }
        
        int start = max, end = sum;
        while(start < end){
            int mid = (start + end) >>> 1;
            if(max_sum_can_be(mid,m, nums)) end = mid;
            else start = mid + 1;
        }
        
        return start;
    }
    
    public boolean max_sum_can_be(int target, int m, int[] nums){
        int sum = 0, split = 0;
        
        for(int num: nums){
            if(sum + num > target){
                split++;
                sum = num;
                if(split > m) return false;
            }else sum += num;
        }
        
        if(sum > 0 && split + 1 > m) return false;
        return true;
    }
}
```