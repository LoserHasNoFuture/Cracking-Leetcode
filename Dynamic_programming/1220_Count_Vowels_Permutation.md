# 1220. Count Vowels Permutation
Given an integer  `n`, your task is to count how many strings of length  `n`  can be formed under the following rules:

-   Each character is a lower case vowel (`'a'`,  `'e'`,  `'i'`,  `'o'`,  `'u'`)
-   Each vowel `'a'`  may only be followed by an  `'e'`.
-   Each vowel `'e'`  may only be followed by an  `'a'` or an  `'i'`.
-   Each vowel `'i'`  **may not**  be followed by another  `'i'`.
-   Each vowel `'o'`  may only be followed by an  `'i'`  or a `'u'`.
-   Each vowel `'u'`  may only be followed by an  `'a'.`

Since the answer may be too large, return it modulo  `10^9 + 7.`

**Example 1:**

**Input:** n = 1
**Output:** 5
**Explanation:** All possible strings are: "a", "e", "i" , "o" and "u".

**Example 2:**

**Input:** n = 2
**Output:** 10
**Explanation:** All possible strings are: "ae", "ea", "ei", "ia", "ie", "io", "iu", "oi", "ou" and "ua".

**Example 3:**

**Input:** n = 5
**Output:** 68

**Constraints:**

-   `1 <= n <= 2 * 10^4`

# Solution 1: DP (Beat 76%)
```
class Solution {
    public int countVowelPermutation(int n) {
        boolean[][] mat = new boolean[][]{{false,true,false,false,false},
                                          {true,false,true,false,false},
                                          {true,true,false,true,true},
                                          {false,false,true,false,true},
                                          {true,false,false,false,false}};
        
        int[] dp = new int[]{1,1,1,1,1};
        int MOD = (int) (1e9 + 7);
        
        for(int i = 1; i < n; i++){
            int[] next = new int[5];
            for(int j = 0; j < 5; j++){
                for(int k = 0; k < 5; k++){
                    if(mat[k][j]) next[j] = (next[j]+dp[k])% MOD;
                }
            }
            dp = next;
        }
        
        int res = 0;
        for(int cnt: dp) res = (res + cnt)% MOD;
        return res;
    }
}
```

