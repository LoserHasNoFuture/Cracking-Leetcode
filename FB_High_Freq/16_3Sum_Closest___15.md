# 16. 3Sum Closest
Given an array  `nums`  of  _n_  integers and an integer  `target`, find three integers in  `nums` such that the sum is closest to `target`. Return the sum of the three integers. You may assume that each input would have exactly one solution.

**Example 1:**

**Input:** nums = [-1,2,1,-4], target = 1
**Output:** 2
**Explanation:** The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

**Constraints:**

-   `3 <= nums.length <= 10^3`
-   `-10^3 <= nums[i] <= 10^3`
-   `-10^4 <= target <= 10^4`

# Solution 1: Similar to 3-Sum (Beat 100%)
```
class Solution {
    int diff = Integer.MAX_VALUE, res = 0;
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        
        for(int i = 0; i <= nums.length - 3; i++){
            if(diff == 0) break;
            two_sum(nums,i+1,target-nums[i]);
        }
        
        System.out.println(diff);
        return res;
    }
    
    public void two_sum(int[] nums, int index, int target){
        int start = index, end = nums.length - 1;
        
        while(start < end){
            if(Math.abs(nums[start] + nums[end] - target) < diff){
                diff = Math.abs(nums[start] + nums[end] - target);
                res = nums[start] + nums[end] + nums[index-1];
                if(diff == 0) break;
            }

            if(nums[start] + nums[end] < target) start++;
            else end--;
        }
        
    }
}
```