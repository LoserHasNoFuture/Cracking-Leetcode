# 387. First Unique Character in a String
Given a string  `s`,  _find the first non-repeating character in it and return its index_. If it does not exist, return  `-1`.

**Example 1:**

**Input:** s = "leetcode"
**Output:** 0

**Example 2:**

**Input:** s = "loveleetcode"
**Output:** 2

**Example 3:**

**Input:** s = "aabb"
**Output:** -1

**Constraints:**

-   `1 <= s.length <= 105`
-   `s`  consists of only lowercase English letters.

# Solution 1: Two Pass
第一遍统计个数
第二遍找答案

# Solution 2: Two Pointers
```
class Solution {
    public int firstUniqChar(String s) {
        if(s.length() == 0) return -1;
        int fast = 0, slow = 0, n = s.length();
        int[] count = new int[256];
        
        while(fast < n){
            count[s.charAt(fast++)]++;
            while(slow < n && count[s.charAt(slow)] > 1) slow++;
            if(slow == n) return -1;
        }
        
        return slow;
    }
}
```

# Solution 3: One Pass
```
class Solution {
    public int firstUniqChar(String s) {
        boolean[] set = new boolean[256];
        LinkedHashMap<Character,Integer> map = new LinkedHashMap<Character,Integer>();
        
        for(int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            if(set[c]) map.remove(c);
            else{
                set[c] = true;
                map.put(c, i);
            }
        }
        
        if(map.size() == 0) return -1;
        return map.entrySet().iterator().next().getValue();
    }
}
```
