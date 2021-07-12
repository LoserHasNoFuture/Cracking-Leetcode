# 91. Decode Ways 639
A message containing letters from  `A-Z`  can be  **encoded**  into numbers using the following mapping:

'A' -> "1"
'B' -> "2"
...
'Z' -> "26"

To  **decode**  an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example,  `"11106"`  can be mapped into:

-   `"AAJF"`  with the grouping  `(1 1 10 6)`
-   `"KJF"`  with the grouping  `(11 10 6)`

Note that the grouping  `(1 11 06)`  is invalid because  `"06"`  cannot be mapped into  `'F'`  since  `"6"`  is different from  `"06"`.

Given a string  `s`  containing only digits, return  _the  **number**  of ways to  **decode**  it_.

The answer is guaranteed to fit in a  **32-bit**  integer.

**Example 1:**

**Input:** s = "12"
**Output:** 2
**Explanation:** "12" could be decoded as "AB" (1 2) or "L" (12).

**Example 2:**

**Input:** s = "226"
**Output:** 3
**Explanation:** "226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).

**Example 3:**

**Input:** s = "0"
**Output:** 0
**Explanation:** There is no character that is mapped to a number starting with 0.
The only valid mappings with 0 are 'J' -> "10" and 'T' -> "20", neither of which start with 0.
Hence, there are no valid ways to decode this since all digits need to be mapped.

**Example 4:**

**Input:** s = "06"
**Output:** 0
**Explanation:** "06" cannot be mapped to "F" because of the leading zero ("6" is different from "06").

**Constraints:**

-   `1 <= s.length <= 100`
-   `s`  contains only digits and may contain leading zero(s).

# Solution 1: BackTracking + Memo 
```
class Solution {
    char[] word;
    int[] memo;
    public int numDecodings(String s) {
        word = s.toCharArray();
        memo = new int[s.length()];
        Arrays.fill(memo,-1);
        int res = dfs(0);
        return res;
    }
    
    public int dfs(int index){
        if(index == word.length) return 1;
        if(word[index] == '0') return 0;
        if(memo[index] != -1) return memo[index];
        
        int cnt = dfs(index+1);
        
        if(index + 1 < word.length){
            int val = Integer.parseInt("" + word[index] + word[index+1]);
            if(val <= 26) cnt += dfs(index+2);
        }
        
        memo[index] = cnt;
        return cnt;
    }
}
```

# Solution 2: Dynamic Programming (Can reduce to O(1) Space)
```
class Solution {
    
    public int numDecodings(String s) {
        int n = s.length();
        int[] memo = new int[n+1];
        
        memo[n] = 1;
        for(int i = n - 1; i >= 0; i--){
            if(s.charAt(i) == '0') memo[i] = 0;
            else{
                memo[i] = memo[i+1];
                if(i + 2 <= n && Integer.parseInt(s.substring(i,i+2)) <= 26) 
                    memo[i] += memo[i+2];
            }
        }
        
        return memo[0];
    }
    
}
```

