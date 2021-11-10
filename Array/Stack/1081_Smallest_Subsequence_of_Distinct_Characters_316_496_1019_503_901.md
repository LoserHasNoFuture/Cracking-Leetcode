# 1081. Smallest Subsequence of Distinct Characters 316 496，1019，503, 901 

Given a string  `s`, return  _the lexicographically smallest subsequence of_  `s`  _that contains all the distinct characters of_  `s`  _exactly once_.

**Example 1:**

**Input:** s = "bcabc"
**Output:** "abc"

**Example 2:**

**Input:** s = "cbacdcbc"
**Output:** "acdb"

**Constraints:**

-   `1 <= s.length <= 1000`
-   `s`  consists of lowercase English letters.

**Note:** This question is the same as 316: [https://leetcode.com/problems/remove-duplicate-letters/](https://leetcode.com/problems/remove-duplicate-letters/)

# Solution 1: Monotonic Stack O(n) Time and O(1) Space
```
class Solution {
    public String smallestSubsequence(String s) {
        int n = s.length(), index = -1;
        int[] last = new int[26];
        boolean[] occupied = new boolean[26];
        char[] stack = new char[26]; // because each character only appear once.
        for(int i = 0; i < n; i++) last[s.charAt(i)-'a'] = i;
        
        for(int i = 0; i < n; i++){
            char c = s.charAt(i);
            if(occupied[c-'a']) continue;
            while(index >= 0 && i <= last[stack[index]-'a'] && c < stack[index]){
                occupied[stack[index]-'a'] = false;
                index--;
            }
            stack[++index] = c;
            occupied[c-'a'] = true;
        }
        
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i <= index; i++){
            sb.append(stack[i]);
        }
        
        return sb.toString();
    }
}
```