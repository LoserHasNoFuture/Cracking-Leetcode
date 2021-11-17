# 3. Longest Substring Without Repeating Characters
Given a string  `s`, find the length of the  **longest substring**  without repeating characters.

**Example 1:**

**Input:** s = "abcabcbb"
**Output:** 3
**Explanation:** The answer is "abc", with the length of 3.

**Example 2:**

**Input:** s = "bbbbb"
**Output:** 1
**Explanation:** The answer is "b", with the length of 1.

**Example 3:**

**Input:** s = "pwwkew"
**Output:** 3
**Explanation:** The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

**Example 4:**

**Input:** s = ""
**Output:** 0

**Constraints:**

-   `0 <= s.length <= 5 * 104`
-   `s`  consists of English letters, digits, symbols and spaces.

# Solution: Sliding Window
```
class Solution {
    public int lengthOfLongestSubstring(String s) {
        boolean[] set = new boolean[256];
        int right = 0, left = 0, res = 0;
        
        while(right < s.length()){
            char c = s.charAt(right);
            right++;
            
            while(set[c]){
                char tmp = s.charAt(left);
                left++;
                set[tmp] = false;
            }
            
            set[c] = true;
            res = Math.max(res, right - left);
        }
        
        return res;
    }
}
```

Better Coding Skills:
```
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int[] pos = new int[256];
        int left = 0, right = 0, max = 0;
        Arrays.fill(pos,-1);
        
        while(right < s.length()){
            char c = s.charAt(right);
            if(pos[c] >= left){
                max = Math.max(max, right-left);
                left = pos[c] + 1;
            }
            pos[c] = right;
            right++;
        }
        max = Math.max(max, right-left);
       
        
        return max;
    }
}


```