# 1256. Encode Number
Given a non-negative integer  `num`, Return its  _encoding_  string.

The encoding is done by converting the integer to a string using a secret function that you should deduce from the following table:

![](https://assets.leetcode.com/uploads/2019/06/21/encode_number.png)

**Example 1:**

**Input:** num = 23
**Output:** "1000"

**Example 2:**

**Input:** num = 107
**Output:** "101100"

**Constraints:**

-   `0 <= num <= 10^9`
# Solution:
```
class Solution {
    public String encode(int n) {
        
        // 主要就是发现n前面的prefix是 (n-1)/2,  然后后面按照0,1的方式累加
        // 很类似于binary encoding
        if(n == 0) return "";
        return encode((n-1)/2) + (n-1)%2 ;
        
    }
}
```