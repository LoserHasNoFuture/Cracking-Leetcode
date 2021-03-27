# 72. Edit Distance 1143 1035
Given two strings  `word1`  and  `word2`, return  _the minimum number of operations required to convert  `word1`  to  `word2`_.

You have the following three operations permitted on a word:

-   Insert a character
-   Delete a character
-   Replace a character

**Example 1:**

**Input:** word1 = "horse", word2 = "ros"
**Output:** 3
**Explanation:** 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')

**Example 2:**

**Input:** word1 = "intention", word2 = "execution"
**Output:** 5
**Explanation:** 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')

**Constraints:**

-   `0 <= word1.length, word2.length <= 500`
-   `word1`  and  `word2`  consist of lowercase English letters.

# Solution 1: 2-D DP (Beat 89%)
```
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        int[][] dp = new int[m+1][n+1];
        
        
        for(int i = 1; i <= m; i++) dp[i][0] = i;
        for(int j = 1; j <= n; j++) dp[0][j] = j;
        
        
        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                if(word1.charAt(i-1) == word2.charAt(j-1)) dp[i][j] = dp[i-1][j-1];
                else dp[i][j] = Math.min(dp[i-1][j],Math.min(dp[i-1][j-1], dp[i][j-1]))+1;
                // dp[i-1][j-1]: replace
                // dp[i-1][j]: deletion
                // dp[i][j-1]: insertion
            }
        }
        
        return dp[m][n];
    }
}
```

