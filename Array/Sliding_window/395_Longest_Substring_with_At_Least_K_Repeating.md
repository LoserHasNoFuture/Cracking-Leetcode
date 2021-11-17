# 395. Longest Substring with At Least K Repeating Characters
Given a string  `s`  and an integer  `k`, return  _the length of the longest substring of_  `s`  _such that the frequency of each character in this substring is greater than or equal to_  `k`.

**Example 1:**

**Input:** s = "aaabb", k = 3
**Output:** 3
**Explanation:** The longest substring is "aaa", as 'a' is repeated 3 times.

**Example 2:**

**Input:** s = "ababbc", k = 2
**Output:** 5
**Explanation:** The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.

**Constraints:**

-   `1 <= s.length <= 104`
-   `s`  consists of only lowercase English letters.
-   `1 <= k <= 105`

# Solution 1: O(n)
```
class Solution {
    public int longestSubstring(String s, int k) {
        int res = 0, n = s.length();
        if(k == 1) return n;
        for(int distinct = 1; distinct <= 26; distinct++){
            int left = 0, right = 0, valid_cnt = 0;
            HashMap<Character,Integer> map = new HashMap<Character,Integer>();
            
            while(right < n){
                char c = s.charAt(right);
                map.put(c, map.getOrDefault(c,0)+1);
                if(map.get(c) == k) valid_cnt++;
                
                while(map.size() > distinct){
                    char left_c = s.charAt(left++);
                    int left_cnt = map.get(left_c);
                    if(left_cnt == k) valid_cnt--;
                    if(--left_cnt == 0) map.remove(left_c);
                    else map.put(left_c, left_cnt);
                }
                
                if(valid_cnt == distinct && valid_cnt == map.size()) {
                    res = Math.max(res, right-left+1);
                }
                
                right++;
            }
        }
        
        return res;
    }
}
```