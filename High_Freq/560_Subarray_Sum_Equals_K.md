# 560. Subarray Sum Equals K
Given an array of integers  `nums`  and an integer  `k`, return  _the total number of continuous subarrays whose sum equals to  `k`_.

**Example 1:**

**Input:** nums = [1,1,1], k = 2
**Output:** 2

**Example 2:**

**Input:** nums = [1,2,3], k = 3
**Output:** 2

**Constraints:**

-   `1 <= nums.length <= 2 * 104`
-   `-1000 <= nums[i] <= 1000`
-   `-107  <= k <= 107`

# Solution 1: Prefix Sum + HashMap (Beat 100%)
```
class Solution {
    public int subarraySum(int[] nums, int k) {
        if(nums.length == 0) return 0;
        if(nums.length == 1) return (nums[0] == k)? 1: 0;
        int res = 0;

        HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();

        map.put(0,1); 
        int sum = 0;
        for(int i = 0; i < nums.length; i++){
            sum += nums[i];
            
            res += map.getOrDefault(sum-k,0);
            map.put(sum, map.getOrDefault(sum,0)+1);
        }
        
        return res;
    }
}
```