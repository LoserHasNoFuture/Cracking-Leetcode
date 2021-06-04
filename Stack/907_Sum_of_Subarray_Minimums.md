# 907. Sum of Subarray Minimums
Given an array of integers arr, find the sum of  `min(b)`, where  `b`  ranges over every (contiguous) subarray of  `arr`. Since the answer may be large, return the answer  **modulo**  `109  + 7`.

**Example 1:**

**Input:** arr = [3,1,2,4]
**Output:** 17
**Explanation:** 
Subarrays are [3], [1], [2], [4], [3,1], [1,2], [2,4], [3,1,2], [1,2,4], [3,1,2,4]. 
Minimums are 3, 1, 2, 4, 1, 1, 2, 1, 1, 1.
Sum is 17.

**Example 2:**

**Input:** arr = [11,81,94,43,3]
**Output:** 444

**Constraints:**

-   `1 <= arr.length <= 3 * 104`
-   `1 <= arr[i] <= 3 * 104`

# Solution 1: Stack + DP (Beat 100%)
```
class Solution {
   
    public int sumSubarrayMins(int[] arr) {
        if(arr.length == 0) return 0;
        long res = 0, mod = (long)(1e9 + 7);
        int n = arr.length, cur = -1;
        int[] index = new int[n];
        long[] dp = new long[n];
        
        for(int i = 0; i < n; i++){
            long tmp = (long)arr[i];
            while(cur >= 0 && arr[index[cur]] >= arr[i]) cur--;
            tmp = (tmp + (long )arr[i]*(cur >= 0? i - index[cur] - 1: i))%mod;
            index[++cur] = i;
            if(cur != 0) tmp = (tmp + dp[index[cur-1]])%mod;
            dp[i] = tmp;
            res = (res + tmp)%mod;
        }
        
        return (int)res;
    }
}
```

