# 523. Continuous Subarray Sum 974 525 560
  
Given an integer array  `nums`  and an integer  `k`, return  `true`  _if_ `nums` _has a continuous subarray of size  **at least two**  whose elements sum up to a multiple of_  `k`_, or_ `false` _otherwise_.

An integer  `x`  is a multiple of  `k`  if there exists an integer  `n`  such that  `x = n * k`.  `0`  is  **always**  a multiple of  `k`.

**Example 1:**

**Input:** nums = [23,2,4,6,7], k = 6
**Output:** true
**Explanation:** [2, 4] is a continuous subarray of size 2 whose elements sum up to 6.

**Example 2:**

**Input:** nums = [23,2,6,4,7], k = 6
**Output:** true
**Explanation:** [23, 2, 6, 4, 7] is an continuous subarray of size 5 whose elements sum up to 42.
42 is a multiple of 6 because 42 = 7 * 6 and 7 is an integer.

**Example 3:**

**Input:** nums = [23,2,6,4,7], k = 13
**Output:** false

**Constraints:**

-   `1 <= nums.length <= 105`
-   `0 <= nums[i] <= 109`
-   `0 <= sum(nums[i]) <= 231  - 1`
-   `1 <= k <= 231  - 1`

# Solution: Prefix Sum + HashMap
```
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        HashMap<Integer,Integer> remainder = new HashMap<Integer,Integer>();
        int n = nums.length, sum = 0;
        remainder.put(0,-1);
        
        for(int i = 0; i < n; i++){
            sum += nums[i];
            if(remainder.containsKey(sum%k) && i - remainder.get(sum%k) > 1) return true;
            if(!remainder.containsKey(sum%k)) remainder.put(sum%k,i);
        }
        
        return false;
    }
}
```