# 309. Best Time to Buy and Sell Stock with Cooldown
Say you have an array for which the  _i_th  element is the price of a given stock on day  _i_.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

-   You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
-   After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)

**Example:**

**Input:** [1,2,3,0,2]
**Output:** 3 
**Explanation:** transactions = [buy, sell, cooldown, buy, sell]

# Solution 1: One-Dimension DP (Beat 50%)
```
class Solution {
    public int maxProfit(int[] prices) {
        int len = prices.length;
        if(len <= 1) return 0;
        int[][] dp = new int[len+1][2];
        dp[0][1] = Integer.MIN_VALUE;
        
        for(int i = 0; i < len; i++){
            dp[i+1][0] = Math.max(dp[i][0], dp[i][1] + prices[i]);
            dp[i+1][1] = Math.max(dp[i][1], (i > 0? dp[i-1][0] - prices[i]: dp[i][0]-prices[i]));
        }
        
        return dp[len][0];
    }
}
```


# Solution 2: Envolve from solution 1 (Beat 100%)
Using two variables to replace dp array
```
class Solution {
    public int maxProfit(int[] prices) {
        int len = prices.length;
        if(len <= 1) return 0;
        int before_yes = 0, no_stock = 0, with_stock = Integer.MIN_VALUE;
        
        for(int i = 0; i < len; i++){
            int tmp0 = Math.max(no_stock, with_stock + prices[i]);
            int tmp1 = Math.max(with_stock, before_yes - prices[i]);
            before_yes = no_stock;
            no_stock = tmp0;
            with_stock = tmp1;
        }
        
        return no_stock;
    }
}
```