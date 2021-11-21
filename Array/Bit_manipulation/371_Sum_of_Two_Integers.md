# 371. Sum of Two Integers
Given two integers  `a`  and  `b`, return  _the sum of the two integers without using the operators_  `+`  _and_  `-`.

**Example 1:**

**Input:** a = 1, b = 2
**Output:** 3

**Example 2:**

**Input:** a = 2, b = 3
**Output:** 5

**Constraints:**

-   `-1000 <= a, b <= 1000`

# Solution
```
class Solution {
    public int getSum(int a, int b) {
//         "1. 非进位的和为a^b, 进位为(a&b)<<<1
//          2. 循环算，直到进位为0"
        while(b != 0){
            int cin = a&b;
            a = a^b;
            b = cin << 1;
        }
        return a;
    }
}
```