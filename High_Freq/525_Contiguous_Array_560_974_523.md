# 525. Contiguous Array 560 974 523
Given a binary array  `nums`, return  _the maximum length of a contiguous subarray with an equal number of_ `0` _and_ `1`.

**Example 1:**

**Input:** nums = [0,1]
**Output:** 2
**Explanation:** [0, 1] is the longest contiguous subarray with an equal number of 0 and 1.

**Example 2:**

**Input:** nums = [0,1,0]
**Output:** 2
**Explanation:** [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `nums[i]`  is either  `0`  or  `1`.

# Solution: Prefix Sum + HashMap
```
class Solution {
    public int findMaxLength(int[] nums) {
        int n = nums.length, sum = 0, res = 0;
        for(int i = 0; i < n; i ++){
            if(nums[i] == 0) nums[i] = -1;
        }
        HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
        map.put(0,-1);
        
        for(int i = 0; i < n; i++){
            sum += nums[i];
            if(map.containsKey(sum)) res = Math.max(res, i - map.get(sum));
            else map.put(sum, i);
        }
        
        return res;
    }
}
```