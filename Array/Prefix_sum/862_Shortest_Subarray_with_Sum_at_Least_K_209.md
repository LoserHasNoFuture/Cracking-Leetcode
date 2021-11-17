# 862. Shortest Subarray with Sum at Least K 209
Given an integer array  `nums`  and an integer  `k`, return  _the length of the shortest non-empty  **subarray**  of_ `nums` _with a sum of at least_ `k`. If there is no such  **subarray**, return  `-1`.

A  **subarray**  is a  **contiguous**  part of an array.

**Example 1:**

**Input:** nums = [1], k = 1
**Output:** 1

**Example 2:**

**Input:** nums = [1,2], k = 4
**Output:** -1

**Example 3:**

**Input:** nums = [2,-1,2], k = 3
**Output:** 3

**Constraints:**

-   `1 <= nums.length <= 105`
-   `-105  <= nums[i] <= 105`
-   `1 <= k <= 109`


# Solution: Prefix Sum + Monotonic Queue
```
class Solution {
    
    public int shortestSubarray(int[] nums, int target) {
        int n = nums.length, res = Integer.MAX_VALUE;
        Deque<Integer> queue = new ArrayDeque<Integer>();
        int[] sum = new int[n+1];
        
        for(int i = 0; i < n ; i++) {
        // to resolve overflow issue
            if(nums[i] < 0 && sum[i] + nums[i] > sum[i]) 
                sum[i+1] = Integer.MIN_VALUE;
            else if(nums[i] > 0 && sum[i] + nums[i] < sum[i]) 
                sum[i+1] = Integer.MAX_VALUE;
            else sum[i + 1] = sum[i]+nums[i];
        }
            
        
        for(int i = 0; i < n + 1; i++) {
            while(!queue.isEmpty() && sum[queue.peekLast()] >= sum[i]){
                queue.pollLast();
            }
            while(!queue.isEmpty() && sum[i] - sum[queue.peekFirst()] >= target){
                res = Math.min(res, i - queue.pollFirst());
            }
            
            queue.offerLast(i);
        }
        
        return res == Integer.MAX_VALUE? -1: res;
    }
}
```