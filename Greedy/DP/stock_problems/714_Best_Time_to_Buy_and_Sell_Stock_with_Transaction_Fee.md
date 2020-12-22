# 714. Best Time to Buy and Sell Stock with Transaction Fee
Your are given an array of integers  `prices`, for which the  `i`-th element is the price of a given stock on day  `i`; and a non-negative integer  `fee`  representing a transaction fee.

You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction. You may not buy more than 1 share of a stock at a time (ie. you must sell the stock share before you buy again.)

Return the maximum profit you can make.

**Example 1:**  

**Input:** prices = [1, 3, 2, 8, 4, 9], fee = 2
**Output:** 8
**Explanation:** The maximum profit can be achieved by:
-   Buying at prices[0] = 1
-   Selling at prices[3] = 8
-   Buying at prices[4] = 4
-   Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.

**Note:**

-   `0 < prices.length <= 50000`.
-   `0 < prices[i] < 50000`.
-   `0 <= fee < 50000`.

# Solution 1: One-Dimension DP (Beat 14%)
```
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int len = prices.length;
        if(len <= 1) return 0;
        int[][] dp = new int[len+1][2];
        dp[0][1] = Integer.MIN_VALUE;
        
        for(int i = 0; i < len; i++){
            dp[i+1][0] = Math.max(dp[i][0], dp[i][1]+prices[i]);
            dp[i+1][1] = Math.max(dp[i][1], dp[i][0]-prices[i]-fee);
        }
        
        return dp[len][0];
    }
}
```


# Solution 2: Envolve From Solution 1 (Beat 92%)
Using two variables to replace dp array
```
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int len = prices.length;
        if(len <= 1) return 0;
        int no_stock = 0, with_stock = Integer.MIN_VALUE;
        
        for(int i = 0; i < len; i++){
            int tmp0 = Math.max(no_stock, with_stock+prices[i]);
            int tmp1 = Math.max(with_stock, no_stock-prices[i]-fee);
            
            no_stock = tmp0;
            with_stock = tmp1;
        }
        
        return no_stock;
    }
}
```