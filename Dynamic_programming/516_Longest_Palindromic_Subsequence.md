# 516. Longest Palindromic Subsequence
Given a string  `s`, find  _the longest palindromic  **subsequence**'s length in_  `s`.

A  **subsequence**  is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** s = "bbbab"
**Output:** 4
**Explanation:** One possible longest palindromic subsequence is "bbbb".

**Example 2:**

**Input:** s = "cbbd"
**Output:** 2
**Explanation:** One possible longest palindromic subsequence is "bb".

**Constraints:**

-   `1 <= s.length <= 1000`
-   `s`  consists only of lowercase English letters.

# Solution 1: DP (Beat 100%)
```
class Solution {
    
    public int longestPalindromeSubseq(String s) {
        int[] dp = new int[s.length()];
        
        for(int i = s.length()-1; i >= 0; i--){
            int[] tmp = new int[s.length()];
            tmp[i] = 1;
            for(int j = i+1; j < s.length(); j++){
                if(s.charAt(i) == s.charAt(j)) tmp[j] = dp[j-1] + 2;
                else tmp[j] = Math.max(dp[j],tmp[j-1]);
            }
            dp = tmp;
        }
        
        return dp[s.length()-1];
    }
   
}
```