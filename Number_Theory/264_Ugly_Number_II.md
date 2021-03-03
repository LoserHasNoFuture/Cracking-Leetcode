# 264. Ugly Number II
Write a program to find the  `n`-th ugly number.

Ugly numbers are **positive numbers**  whose prime factors only include  `2, 3, 5`.

**Example:**

**Input:** n = 10
**Output:** 12
**Explanation:** `1, 2, 3, 4, 5, 6, 8, 9, 10, 12` is the sequence of the first `10` ugly numbers.

**Note:**

1.  `1`  is typically treated as an ugly number.
2.  `n`  **does not exceed 1690**.

# Solution 1: Beat 100%
Ref: [https://leetcode.com/problems/ugly-number-ii/discuss/69362/O(n)-Java-solution](https://leetcode.com/problems/ugly-number-ii/discuss/69362/O(n)-Java-solution)
```
public class Solution {
    public int nthUglyNumber(int n) {
        int[] ugly = new int[n];
        ugly[0] = 1;
        int index2 = 0, index3 = 0, index5 = 0;
        int factor2 = 2, factor3 = 3, factor5 = 5;
        for(int i=1;i<n;i++){
            int min = Math.min(Math.min(factor2,factor3),factor5);
            ugly[i] = min;
            if(factor2 == min)
                factor2 = 2*ugly[++index2];
            if(factor3 == min)
                factor3 = 3*ugly[++index3];
            if(factor5 == min)
                factor5 = 5*ugly[++index5];
        }
        return ugly[n-1];
    }
}
```