# 123. Best Time to Buy and Sell Stock III
Say you have an array for which the  _i_th  element is the price of a given stock on day  _i_.

Design an algorithm to find the maximum profit. You may complete at most  _two_  transactions.

**Note:** You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

**Example 1:**

**Input:** prices = [3,3,5,0,0,3,1,4]
**Output:** 6
**Explanation:** Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.

**Example 2:**

**Input:** prices = [1,2,3,4,5]
**Output:** 4
**Explanation:** Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.

**Example 3:**

**Input:** prices = [7,6,4,3,1]
**Output:** 0
**Explanation:** In this case, no transaction is done, i.e. max profit = 0.

**Example 4:**

**Input:** prices = [1]
**Output:** 0

**Constraints:**

-   `1 <= prices.length <= 105`
-   `0 <= prices[i] <= 105`

# Solution 1: 3-Dimension DP (Beat 15%)
```
class Solution {
    public int maxProfit(int[] prices) {
        int len = prices.length;
        if(len <= 1) return 0;
        int[][][] dp = new int[len+1][2][3];
        for(int k = 0; k < 3; k++) dp[0][1][k] = Integer.MIN_VALUE;
        dp[0][0][0] = 0;
        
        for(int i = 0; i < len; i++){
            for(int k = 1; k < 3; k++){
                dp[i+1][1][k] = Math.max(dp[i][1][k], dp[i][0][k-1]-prices[i]);
                dp[i+1][0][k] = Math.max(dp[i][0][k], dp[i][1][k]+prices[i]);
            }
        }
        
        return Math.max(dp[len][0][2],Math.max(dp[len][0][1],0));
    }
}
```


# Solution 2: 2-Dimension DP (Beat 57%)
```
class Solution {
    public int maxProfit(int[] prices) {
        int len = prices.length;
        if(len <= 1) return 0;
        int[][] dp = new int[2][3];
        for(int k = 0; k < 3; k++) dp[1][k] = Integer.MIN_VALUE;
        
        for(int i = 0; i < len; i++){
            for(int k = 1; k < 3; k++){
                int tmp0 = Math.max(dp[0][k], dp[1][k]+prices[i]);
                int tmp1 = Math.max(dp[1][k], dp[0][k-1]-prices[i]);
                dp[0][k] = tmp0;
                dp[1][k] = tmp1;
            }
        }
        
        return dp[0][2];
    }
}
```

# Solution 3: Only With 4 Variables (Beat 99%)
```
class Solution {
    public int maxProfit(int[] prices) {
        int len = prices.length;
        if(len <= 1) return 0;
        
        int with_stock1 = Integer.MIN_VALUE, with_stock2 = Integer.MIN_VALUE;
        int no_stock1 = 0, no_stock2 = 0;
        
        for(int i = 0; i < len; i++){
            no_stock2 = Math.max(no_stock2, with_stock2 + prices[i]);
            with_stock2 = Math.max(with_stock2, no_stock1 - prices[i]);
            
            no_stock1 = Math.max(no_stock1, with_stock1 + prices[i]);
            with_stock1 = Math.max(with_stock1,  - prices[i]);
        }
        
        return no_stock2;
    }
}
```