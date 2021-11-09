# 96. Unique Binary Search Trees
Given an integer  `n`, return  _the number of structurally unique  **BST'**s (binary search trees) which has exactly_ `n` _nodes of unique values from_  `1`  _to_  `n`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

**Input:** n = 3
**Output:** 5

**Example 2:**

**Input:** n = 1
**Output:** 1

**Constraints:**

-   `1 <= n <= 19`

# Solution 1: DP (Beat 100%)
```
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n+1];
        dp[1] = 1; dp[0] = 1;
        
        for(int i = 2; i <= n; i++){
            for(int j = 1; j <= i/2; j++){
                dp[i] = dp[i] + 2*(dp[j-1]*dp[i-j]);
            }
            if(i%2 == 1) dp[i] += dp[i/2]*dp[i/2];
        }
        
        return dp[n];
    }
}
```