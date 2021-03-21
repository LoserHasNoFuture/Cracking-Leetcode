# 188. Best Time to Buy and Sell Stock IV
You are given an integer array  `prices`  where  `prices[i]` is the price of a given stock on the  `ith`  day.

Design an algorithm to find the maximum profit. You may complete at most  `k`  transactions.

**Notice**  that you may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

**Input:** k = 2, prices = [2,4,1]
**Output:** 2
**Explanation:** Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.

**Example 2:**

**Input:** k = 2, prices = [3,2,6,5,0,3]
**Output:** 7
**Explanation:** Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4. Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.

**Constraints:**

-   `0 <= k <= 109`
-   `0 <= prices.length <= 1000`
-   `0 <= prices[i] <= 1000`

# Solution 1: One-Dimension DP (Beat 96%)
```
class Solution {
    public int maxProfit(int K, int[] prices) {
        int len = prices.length;
        if(len <= 1) return 0;
        int[][][] dp = new int[len+1][2][K+1];
        for(int k = 0; k < K+1; k++) dp[0][1][k] = Integer.MIN_VALUE;

        for(int i = 0; i < len; i++){
            for(int k = 1; k < K+1; k++){
                dp[i+1][1][k] = Math.max(dp[i][1][k], dp[i][0][k-1]-prices[i]);
                dp[i+1][0][k] = Math.max(dp[i][0][k], dp[i][1][k]+prices[i]);
            }
        }
        
        return dp[len][0][K];
    }
    
}
```