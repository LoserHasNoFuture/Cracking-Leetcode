# 69. Sqrt(x)
Implement  `int sqrt(int x)`.

Compute and return the square root of  _x_, where _x_ is guaranteed to be a non-negative integer.

Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.

**Example 1:**

**Input:** 4
**Output:** 2

**Example 2:**

**Input:** 8
**Output:** 2

**Explanation:** The square root of 8 is 2.82842..., and since 
             the decimal part is truncated, 2 is returned.

# Solution 1: Newton's Method
```
class Solution {
    public int mySqrt(int num) {
        if (num <= 1) return num;
        int temp = num/2;
        while(temp > num/temp){
            temp = (temp + num/temp)/2;
        }
        return temp;
    }
}
```

# Solution 2: Binary Search
```
class Solution {
    public int mySqrt(int num) {
        if (num <= 1) return num;
        int start = 1;
        int end = num/2;
        
        while(start < end){
            int mid = (start + end + 1) >>> 1;
            if(num/mid == mid) return mid;
            if(num/mid > mid) start = mid;
            else end = mid - 1;
        }
        
        return start;
        
    }
}
```

# Solution 3: Bit Manipulation
Refer to: [https://leetcode.com/problems/sqrtx/discuss/25048/Share-my-O(log-n)-Solution-using-bit-manipulation](https://leetcode.com/problems/sqrtx/discuss/25048/Share-my-O(log-n)-Solution-using-bit-manipulation)
```
public class Solution {
     public int mySqrt(int x) {
        int res = 0;
        for (int mask = 1 << 15; mask != 0; mask >>>= 1) {
            int next = res | mask; //set bit
            if (next <= x / next) res = next;
        }
        return res;
    }
}
```