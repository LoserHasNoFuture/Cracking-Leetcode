# 696. Count Binary Substrings
Give a binary string  `s`, return the number of non-empty substrings that have the same number of  `0`'s and  `1`'s, and all the  `0`'s and all the  `1`'s in these substrings are grouped consecutively.

Substrings that occur multiple times are counted the number of times they occur.

**Example 1:**

**Input:** s = "00110011"
**Output:** 6
**Explanation:** There are 6 substrings that have equal number of consecutive 1's and 0's: "0011", "01", "1100", "10", "0011", and "01".
Notice that some of these substrings repeat and are counted the number of times they occur.
Also, "00110011" is not a valid substring because all the 0's (and 1's) are not grouped together.

**Example 2:**

**Input:** s = "10101"
**Output:** 4
**Explanation:** There are 4 substrings: "10", "01", "10", "01" that have equal number of consecutive 1's and 0's.

**Constraints:**

-   `1 <= s.length <= 105`
-   `s[i]`  is either  `'0'`  or  `'1'`.

# Solution 1: Count One and Zeros (Beat 100%)
```
class Solution {
    public int countBinarySubstrings(String s) {
        if(s.length() == 0) return 0;
        int res = 0, zeros = 0, ones = 0, p0 = 0, p1 = 0;
        for(char c:s.toCharArray()){
            if(c == '0') {
                zeros++; 
                if(ones != 0){ p1 = ones; ones = 0;}
                if(zeros <= p1) res++;
            }else {
                ones++;
                if(zeros != 0){ p0 = zeros; zeros = 0;}
                if(ones <= p0) res++;
            }
        }
        
        return res;
    }
}
```

# Solution 2: Using One Pointer (Beat 100%)
```
class Solution {
    public int countBinarySubstrings(String s) {
        int cnt = 0, index = 0, last = 0, res = 0;
        char c;
        while(index < s.length()){
            c = s.charAt(index);
            cnt = 1;
            for(++index ;index < s.length() && s.charAt(index) == c; index++) cnt++;
            res += Math.min(last,cnt);
            last = cnt;
        }
        
        return res;
    }
}
```