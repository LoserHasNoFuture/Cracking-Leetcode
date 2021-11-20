# 424. Longest Repeating Character Replacement
You are given a string  `s`  and an integer  `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most  `k`  times.

Return  _the length of the longest substring containing the same letter you can get after performing the above operations_.

**Example 1:**

**Input:** s = "ABAB", k = 2
**Output:** 4
**Explanation:** Replace the two 'A's with two 'B's or vice versa.

**Example 2:**

**Input:** s = "AABABBA", k = 1
**Output:** 4
**Explanation:** Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.

**Constraints:**

-   `1 <= s.length <= 105`
-   `s`  consists of only uppercase English letters.
-   `0 <= k <= s.length`

# Solution: Sliding Window
```
class Solution {
    public int characterReplacement(String s, int k) {
        int[] cnt = new int[26];
        int left = 0, right = 0, res = 0, n = s.length();
        
        while(right < n){
            char c = s.charAt(right);
            cnt[c-'A']++;
            
            while(find_max(cnt) + k  < right - left + 1){
                cnt[s.charAt(left++)-'A']--;
            }
            
            res = Math.max(res, right-left+1);
            right++;
        }
        
        return res;
    }
    
    public int find_max(int[] cnt){
        int max = 0;
        for(int num: cnt){
            if(num > max) max = num;
        }
        return max;
    }
}
```

