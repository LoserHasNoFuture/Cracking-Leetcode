# 322. Coin Change
You are given coins of different denominations and a total amount of money  _amount_. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return  `-1`.

**Example 1:**

**Input:** coins = `[1, 2, 5]`, amount = `11`
**Output:** `3` 
**Explanation:** 11 = 5 + 5 + 1

**Example 2:**

**Input:** coins = `[2]`, amount = `3`
**Output:** -1

**Note**:  
You may assume that you have an infinite number of each kind of coin.

# Solution: DP
```
class Solution {
    public int coinChange(int[] coins, int amount) {
        if(amount == 0) return 0;
        if(coins.length == 0 || amount < 0) return -1;        
        int[] dp = new int[amount+1];
        dp[0] = 0;
        
        for(int i = 1; i <= amount; i++){
            for(int j = 0; j < coins.length; j++){
                if(i-coins[j] == 0) dp[i] = 1;
                else if (i-coins[j] < 0) continue;
                else{
                    if(dp[i-coins[j]] == 0) continue;
                    if(dp[i] == 0) dp[i] = dp[i-coins[j]] + 1;
                    else dp[i] = Math.min(dp[i-coins[j]] + 1,dp[i]);
                }
            }
        }
        return dp[amount] == 0? -1: dp[amount];
    }
}
```
Same Idea, Better Coding Skill:
Refer From: [https://leetcode.com/problems/coin-change/discuss/77368/*Java*-Both-iterative-and-recursive-solutions-with-explanations](https://leetcode.com/problems/coin-change/discuss/77368/*Java*-Both-iterative-and-recursive-solutions-with-explanations)
```
public class Solution {
    public int coinChange(int[] coins, int amount) {
        if(amount<1) return 0;
        int[] dp = new int[amount+1];
        int sum = 0;

        while(++sum<=amount) {
            int min = -1;
            for(int coin : coins) {
                if(sum >= coin && dp[sum-coin]!=-1) {
                    int temp = dp[sum-coin]+1;
                    min = min<0 ? temp : (temp < min ? temp : min);
                }
            }
            dp[sum] = min;
        }
        return dp[amount];
    }
}
```

# Solution: Recursion 
Too Straightforward.  