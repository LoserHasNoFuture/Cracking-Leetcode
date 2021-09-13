# 877. Stone Game
Alex and Lee play a game with piles of stones. There are an even number of piles  **arranged in a row**, and each pile has a positive integer number of stones  `piles[i]`.

The objective of the game is to end with the most stones. The total number of stones is odd, so there are no ties.

Alex and Lee take turns, with Alex starting first. Each turn, a player takes the entire pile of stones from either the beginning or the end of the row. This continues until there are no more piles left, at which point the person with the most stones wins.

Assuming Alex and Lee play optimally, return  `True` if and only if Alex wins the game.

**Example 1:**

**Input:** piles = [5,3,4,5]
**Output:** true
**Explanation:** 
Alex starts first, and can only take the first 5 or the last 5.
Say he takes the first 5, so that the row becomes [3, 4, 5].
If Lee takes 3, then the board is [4, 5], and Alex takes 5 to win with 10 points.
If Lee takes the last 5, then the board is [3, 4], and Alex takes 4 to win with 9 points.
This demonstrated that taking the first 5 was a winning move for Alex, so we return true.

**Constraints:**

-   `2 <= piles.length <= 500`
-   `piles.length`  is even.
-   `1 <= piles[i] <= 500`
-   `sum(piles)`  is odd.

# Solution 1: DP
Refer From: [https://leetcode.com/problems/stone-game/discuss/154610/DP-or-Just-return-true](https://leetcode.com/problems/stone-game/discuss/154610/DP-or-Just-return-true)

`dp[i][j]`  means the biggest number of stones you can get more than opponent picking piles in  `piles[i] ~ piles[j]`.  
You can first pick  `piles[i]`  or  `piles[j]`.

1.  If you pick  `piles[i]`, your result will be  `piles[i] - dp[i + 1][j]`
2.  If you pick  `piles[j]`, your result will be  `piles[j] - dp[i][j - 1]`

So we get:  
`dp[i][j] = max(piles[i] - dp[i + 1][j], piles[j] - dp[i][j - 1])`  
We start from smaller subarray and then we use that to calculate bigger subarray.

```
class Solution {
    public boolean stoneGame(int[] piles) {
        int len = piles.length;
        if(len == 2) return true;
        int[][] dp = new int[len][len];
        
        for(int i = 0; i < len; i++) dp[i][i] = piles[i];
        
        for(int k = 1; k < len; k++){
            for(int i = 0; i+k < len; i++){
                dp[i][i+k] = Math.max(piles[i]-dp[i+1][i+k], piles[i+k]-dp[i][i+k-1]);
            }
        }
        
        return dp[0][len-1] > 0;
    }
}
```

Condense to 1 dimension DP:
```
class Solution {
    public boolean stoneGame(int[] piles) {
        int len = piles.length;
        if(len == 2) return true;
        int[] dp = new int[len];        
        for(int i = 0; i < len; i++) dp[i] = piles[i];
        
        for(int k = 1; k < len; k++){
            for(int i = len-1; i >= k; i--){
                dp[i] = Math.max(piles[i]-dp[i-1],piles[i-k]-dp[i]);
            }
        }

        return  dp[len-1] > 0;
    }
}
```

# Solution 2: Tricky Solution
This solution can be only applied to the case when there are even number of piles.

Refer from the same link.
```
class Solution {
    public boolean stoneGame(int[] piles) {
//         Two things:
//         If Alice wants, she can get all even/odd piles, and force Bob to get the other (odd/even).
//         There is no tie.
        
//         Alice can calculate odd/even piles value, and choose the larger one.
        return true;
    }
}
```
