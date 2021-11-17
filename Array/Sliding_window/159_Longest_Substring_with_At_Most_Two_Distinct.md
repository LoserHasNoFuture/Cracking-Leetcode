# 159. Longest Substring with At Most Two Distinct Characters
Given a string  **_s_**  , find the length of the longest substring **_t_** that contains  **at most** 2 distinct characters.

**Example 1:**

**Input:** "eceba"
**Output:** 3
**Explanation: _t_**  is "ece" which its length is 3.

**Example 2:**

**Input:** "ccaabbb"
**Output:** 5
**Explanation: _t_**  is "aabbb" which its length is 5.

# Solution: Sliding Window
```
class Solution {
    public int lengthOfLongestSubstringTwoDistinct(String s) {
        if(s.length() <= 2) return s.length();
        
        // key: character, value: last position
        HashMap<Character,Integer> map = new HashMap<Character,Integer>();
        
        int start = 0; int end = 0; int max = 0;
        while(end < s.length()){
            char cur = s.charAt(end);
            if(!map.containsKey(cur) && map.size() == 2){
                start = end;
                for(int i : map.values()){
                    start = Math.min(start,i);
                }
                map.remove(s.charAt(start));
                start++;
            }
            map.put(cur,end);
            max = Math.max(max, end-start+1);
            end++;
        }
        
        return max;
    }
}
```