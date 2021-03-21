# 122. Best Time to Buy and Sell Stock II
Say you have an array  `prices`  for which the  _i_th  element is the price of a given stock on day  _i_.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

**Note:**  You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

**Example 1:**

**Input:** [7,1,5,3,6,4]
**Output:** 7
**Explanation:** Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
             Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.

**Example 2:**

**Input:** [1,2,3,4,5]
**Output:** 4
**Explanation:** Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
             Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
             engaging multiple transactions at the same time. You must sell before buying again.

**Example 3:**

**Input:** [7,6,4,3,1]
**Output:** 0
**Explanation:** In this case, no transaction is done, i.e. max profit = 0.

**Constraints:**

-   `1 <= prices.length <= 3 * 10 ^ 4`
-   `0 <= prices[i] <= 10 ^ 4`


# Solution 1: Greedy (Beat 89%)
```
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length <= 1) return 0;
        int profit = 0;
        int buy = prices[0];
        int ptr = 1;
        
        while(ptr < prices.length){
            while(ptr < prices.length && prices[ptr] > prices[ptr-1]) ptr++;
            profit += (prices[ptr - 1] - buy);
            if(ptr < prices.length) buy = prices[ptr];
            ptr++;
        }
        
        return profit;
    }
}
```


# Solution 2: One-Dimension DP (Beat 6%)
```
class Profit{
    int with_stock;
    int no_stock;
    
    Profit(){
        this.with_stock = Integer.MIN_VALUE;
        this.no_stock = 0;
    }
}

class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length == 1) return 0;
        int[][] dp = new int[prices.length+1][2];
        dp[0][1] = Integer.MIN_VALUE;
        for(int i = 0 ; i < prices.length; i++){
            dp[i+1][1] = Math.max(dp[i][1], dp[i][0]-prices[i]);
            dp[i+1][0] = Math.max(dp[i][0], dp[i][1] + prices[i]);
        }
        
        return dp[prices.length][0];
    }
}
```


# Solution 3: Envolve From Solution 2 (Beat 95%)
Using two variables to replace dp array.
```
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length == 1) return 0;
        int no_stock = 0, with_stock = Integer.MIN_VALUE;
        for(int i = 0 ; i < prices.length; i++){
            int tmp1 = Math.max(with_stock, no_stock-prices[i]);
            int tmp0 = Math.max(no_stock, with_stock + prices[i]);
            no_stock = tmp0;
            with_stock = tmp1;
        }
        
        return no_stock;
    }
}
```