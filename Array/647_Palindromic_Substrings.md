# 647. Palindromic Substrings
Given a string  `s`, return  _the number of  **palindromic substrings**  in it_.

A string is a  **palindrome**  when it reads the same backward as forward.

A  **substring**  is a contiguous sequence of characters within the string.

**Example 1:**

**Input:** s = "abc"
**Output:** 3
**Explanation:** Three palindromic strings: "a", "b", "c".

**Example 2:**

**Input:** s = "aaa"
**Output:** 6
**Explanation:** Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".

**Constraints:**

-   `1 <= s.length <= 1000`
-   `s`  consists of lowercase English letters.

# Solution 1: O(n^2)
```
public class Solution {
    
    int res = 0;
    public int countSubstrings(String s) {
        if(s.length() == 0 && s == null) return 0;
        
        for(int i = 0; i < s.length(); i++){
            findPalindrom(i,i,s);
            findPalindrom(i,i+1,s);
        }
        return res;
    }
    
    public void findPalindrom(int left, int right, String s){
        for(;left >=0 && right < s.length(); left--, right++){
            if(s.charAt(left) != s.charAt(right)) break;
            res++;
        }
    }
}
```

# Solution 2: DP
```
public class Solution {
    
    public int countSubstrings(String s) {
        int n = s.length();
        int res = 0;
        boolean[][] dp = new boolean[n][n];
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                dp[i][j] = s.charAt(i) == s.charAt(j) && (j - i  + 1 < 3 || dp[i + 1][j - 1]);
                if(dp[i][j]) ++res;
            }
        }
        return res;
    }

}
```