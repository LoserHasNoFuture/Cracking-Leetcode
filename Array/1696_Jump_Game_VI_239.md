# 1696. Jump Game VI
You are given a  **0-indexed**  integer array  `nums`  and an integer  `k`.

You are initially standing at index  `0`. In one move, you can jump at most  `k`  steps forward without going outside the boundaries of the array. That is, you can jump from index  `i`  to any index in the range  `[i + 1, min(n - 1, i + k)]`  **inclusive**.

You want to reach the last index of the array (index  `n - 1`). Your  **score**  is the  **sum**  of all  `nums[j]`  for each index  `j`  you visited in the array.

Return  _the  **maximum score**  you can get_.

**Example 1:**

**Input:** nums = [1,-1,-2,4,-7,3], k = 2
**Output:** 7
**Explanation:** You can choose your jumps forming the subsequence [1,-1,4,3] (underlined above). The sum is 7.

**Example 2:**

**Input:** nums = [10,-5,-2,4,0,3], k = 3
**Output:** 17
**Explanation:** You can choose your jumps forming the subsequence [10,4,3] (underlined above). The sum is 17.

**Example 3:**

**Input:** nums = [1,-5,-20,4,-1,3,-6,-3], k = 2
**Output:** 0

**Constraints:**

-   `1 <= nums.length, k <= 105`
-   `-104  <= nums[i] <= 104`

# Solution 1: Inituitive DP (O(nk) Limited Time Exceed) 
```
class Solution {
    public int maxResult(int[] nums, int k) {
        int n = nums.length;
        int[] dp = new int[n];
        Arrays.fill(dp,Integer.MIN_VALUE);
        dp[0] = nums[0];
        
        for(int i = 0; i < n-1; i++){
            for(int j = 1; j <= k && i+j < n; j++){
                dp[i+j] = Math.max(dp[i+j], dp[i] + nums[i+j]);
            }
        }
        
        return dp[n-1];
    }
}
```

# Solution 2: DP + PriorityQueue (O(nlogk) Limited Time Exceed) 
```
class Solution {
    public int maxResult(int[] nums, int k) {
        int n = nums.length;
        int[] dp = new int[n];
        PriorityQueue<Integer> queue = new PriorityQueue<Integer>((a,b)->(b-a));
        
        if(k == 1) return dp[n-1];
        for(int i = 0; i <= 1; i++) {
            dp[i] = dp[0] + nums[i];
            queue.offer(dp[i]);
        }
        
        for(int i = 2; i < n; i++){
            dp[i] = nums[i] + queue.peek();
            queue.offer(dp[i]);
            if(i-k >= 0) queue.remove(dp[i-k]);
        }
        
        return dp[n-1];
    }
}
```

# Solution 3: DP + Deque O(n)
Each element is pushed and polled out of queue for once.
```
class Solution {
    public int maxResult(int[] nums, int k) {
        int n = nums.length;
        int[] dp = new int[n];
        Deque<int[]> deque = new ArrayDeque<int[]>();
        dp[0] = nums[0];
        deque.addLast(new int[]{dp[0],0});
        
        for(int i = 1; i < n; i++){
            while(i - deque.getFirst()[1] > k) deque.pollFirst();
            dp[i] = deque.getFirst()[0] + nums[i];
            while(!deque.isEmpty() && deque.getLast()[0] <= dp[i]) deque.pollLast();
            
            deque.addLast(new int[]{dp[i],i});
        }
        
        return dp[n-1];
    }
}
```