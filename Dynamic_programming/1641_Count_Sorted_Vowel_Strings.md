# 1641. Count Sorted Vowel Strings
Given an integer  `n`, return  _the number of strings of length_ `n` _that consist only of vowels (_`a`_,_ `e`_,_ `i`_,_ `o`_,_ `u`_) and are  **lexicographically sorted**._

A string  `s`  is  **lexicographically sorted**  if for all valid  `i`,  `s[i]`  is the same as or comes before  `s[i+1]`  in the alphabet.

**Example 1:**

**Input:** n = 1
**Output:** 5
**Explanation:** The 5 sorted strings that consist of vowels only are `["a","e","i","o","u"].`

**Example 2:**

**Input:** n = 2
**Output:** 15
**Explanation:** The 15 sorted strings that consist of vowels only are
["aa","ae","ai","ao","au","ee","ei","eo","eu","ii","io","iu","oo","ou","uu"].
Note that "ea" is not a valid string since 'e' comes after 'a' in the alphabet.

**Example 3:**

**Input:** n = 33
**Output:** 66045

**Constraints:**

-   `1 <= n <= 50`

# Solution 1: BackTracking (Beat 16%)
```
class Solution {
    int res = 0;
    public int countVowelStrings(int n) {
        dfs(n,-1,0);
        return res;
    }
    
    public void dfs(int n, int index, int depth){
        if(depth == n) res++;
        
        for(int i = index < 0? 0: index; i <= 4; i++){
            if(depth < n) dfs(n,i,depth+1);
        }
            
    }
}
```

# Solution 2: Dynamic Programming (Beat 100%)
```
class Solution {
    
    public int countVowelStrings(int n) {
        int[] dp = new int[5];
        for(int i = 0; i < 5; i++) dp[i] = 1;
        
        for(int depth = 2; depth <= n; depth++){
            for(int i = 3; i >= 0; i--){
                dp[i] = dp[i+1] + dp[i];
            }
        }
        
        int res = 0;
        for(int i = 0; i < 5; i++) res += dp[i];
        
        return res;
    }
    
}
```