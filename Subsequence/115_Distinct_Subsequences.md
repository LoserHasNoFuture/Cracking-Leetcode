# 115. Distinct Subsequences
Given two strings  `s`  and  `t`, return  _the number of distinct subsequences of  `s`  which equals  `t`_.

A string's  **subsequence**  is a new string formed from the original string by deleting some (can be none) of the characters without disturbing the remaining characters' relative positions. (i.e.,  `"ACE"`  is a subsequence of  `"ABCDE"`  while  `"AEC"`  is not).

It is guaranteed the answer fits on a 32-bit signed integer.

**Example 1:**

**Input:** s = "rabbbit", t = "rabbit"
**Output:** 3
**Explanation:**
As shown below, there are 3 ways you can generate "rabbit" from S.
`**rabb**b**it**`
`**ra**b**bbit**`
`**rab**b**bit**`

**Example 2:**

**Input:** s = "babgbag", t = "bag"
**Output:** 5
**Explanation:**
As shown below, there are 5 ways you can generate "bag" from S.
`**ba**b**g**bag`
`**ba**bgba**g**`
`**b**abgb**ag**`
`ba**b**gb**ag**`
`babg**bag**`

**Constraints:**

-   `1 <= s.length, t.length <= 1000`
-   `s`  and  `t`  consist of English letters.


# Solution 1: Dynamic Programming (Beat 92%)
```
class Solution {
    
    // dp[i][j] records the number of variations it has
    
    //   r a b b b i i t
    // r 1 1 1 1 1 1 1 1
    // a 0 1 1 1 1 1 1 1
    // b 0 0 1 2 3 3 3 3
    // i 0 0 0 0 0 3 6 6
    // t 0 0 0 0 0 0 0 6
    
    public int numDistinct(String s, String t) {
        int m = s.length(), n = t.length();
        int[] dp = new int[m];
        
        for(int i = 0; i < m; i++){
            if(s.charAt(i) == t.charAt(0)) dp[i] = (i == 0?0:dp[i-1])+1;
            else dp[i] = i == 0?0:dp[i-1];
        }
        
        for(int i = 1; i < n; i++){
            int[] tmp = new int[m];
            for(int j = i; j < m; j++){
                if(t.charAt(i) == s.charAt(j)) {
                    tmp[j] = tmp[j-1] + dp[j-1];
                }else tmp[j] = tmp[j-1];
            }
            dp = tmp;
        }
        
        return dp[m-1];
    }
}
```