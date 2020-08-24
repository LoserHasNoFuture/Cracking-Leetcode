# 167. Two Sum II - Input array is sorted
Given an array of integers that is already  **_sorted in ascending order_**, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

**Note:**

-   Your returned answers (both index1 and index2) are not zero-based.
-   You may assume that each input would have  _exactly_  one solution and you may not use the  _same_  element twice.

**Example:**

**Input:** numbers = [2,7,11,15], target = 9
**Output:** [1,2]
**Explanation:** The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.


# Solution 1: Two Pointers
```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] res = {1,2};
        if(nums.length <= 2) return res;
        
        int p1 = 0;
        int p2 = nums.length - 1;
        while(p1<p2){
            if(nums[p1] + nums[p2] == target) {
                res[0] = p1+1;
                res[1] = p2+1;
                return res;
            }
            if(nums[p1] + nums[p2] < target) p1++;
            else p2--;
        }
        
        return res;
    }
}
```