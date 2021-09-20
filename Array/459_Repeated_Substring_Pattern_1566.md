# 459. Repeated Substring Pattern 1566
Given a string  `s`, check if it can be constructed by taking a substring of it and appending multiple copies of the substring together.

**Example 1:**

**Input:** s = "abab"
**Output:** true
**Explanation:** It is the substring "ab" twice.

**Example 2:**

**Input:** s = "aba"
**Output:** false

**Example 3:**

**Input:** s = "abcabcabcabc"
**Output:** true
**Explanation:** It is the substring "abc" four times or the substring "abcabc" twice.

**Constraints:**

-   `1 <= s.length <= 104`
-   `s`  consists of lowercase English letters.

# Solution: StraightFoward
```
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        for(int pLen = 1; pLen <= s.length()/2; pLen++){
            if(s.length()%pLen != 0) continue;
            boolean flag = true;
            for(int i = 0, j = pLen; j < s.length(); i++, j++){
                if(s.charAt(i) != s.charAt(j)) {
                    flag = false;
                    break;
                }
            }
            // System.out.println(pLen);
            if(flag) return true;
        }
        return false;
    }
}
```