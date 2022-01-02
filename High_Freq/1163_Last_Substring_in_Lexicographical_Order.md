# 1163. Last Substring in Lexicographical Order
Given a string  `s`, return  _the last substring of_  `s`  _in lexicographical order_.

**Example 1:**

**Input:** s = "abab"
**Output:** "bab"
**Explanation:** The substrings are ["a", "ab", "aba", "abab", "b", "ba", "bab"]. The lexicographically maximum substring is "bab".

**Example 2:**

**Input:** s = "leetcode"
**Output:** "tcode"

**Constraints:**

-   `1 <= s.length <= 4 * 105`
-   `s`  contains only lowercase English letters.


# Solution: Two Pointers
Refer from: https://leetcode.com/problems/last-substring-in-lexicographical-order/discuss/363662/Short-python-code-O(n)-time-and-O(1)-space-with-proof-and-visualization

```
The problem is essentially finding the optimal starting point start, such that s[start:] is the largest substring. (not s[start : end] because s[start:] >= s[start : end])

Think of two sequences matching k characters so far and only differ at s[i+k] > s[j+k]  
i ... i+k  
j ... j+k

Regardless of j relative position to i, we could set j to j+k+1. This is because for any j2 (j<j2<j+k); and i2 (i<i2<j+k), i+k-i2 = j+k-j2, substring [j2, j+k] is still smaller than [i2, i+k] (these two still only differ at s[i+k] > s[j+k]), so j<j2<j+k can't be the optimal starting point.

Reversely and similarly, if s[i+k] < s[j+k], then set i to i+k+1

at last to break tie, when i==j, set j=j+1;
```

```
class Solution {
    public String lastSubstring(String s) {
        int n = s.length();
        int left = 0, right = 1, cnt = 0;
        
        while(right + cnt < n){
            char c1 = s.charAt(left+cnt), c2 = s.charAt(right+cnt);
            if(c1 == c2){
                cnt++;
                continue;
            }else if(c1 < c2) {
                left = Math.max(left+cnt+1,right);
                right = left + 1;
            }else{
                right = right + cnt + 1; 
            }
            
            cnt = 0;    
        }
        
        return s.substring(left);
    }
}
```