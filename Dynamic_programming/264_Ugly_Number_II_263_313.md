# 264. Ugly Number II
An  **ugly number**  is a positive integer whose prime factors are limited to  `2`,  `3`, and  `5`.

Given an integer  `n`, return  _the_  `nth`  _**ugly number**_.

**Example 1:**

**Input:** n = 10
**Output:** 12
**Explanation:** [1, 2, 3, 4, 5, 6, 8, 9, 10, 12] is the sequence of the first 10 ugly numbers.

**Example 2:**

**Input:** n = 1
**Output:** 1
**Explanation:** 1 has no prime factors, therefore all of its prime factors are limited to 2, 3, and 5.

**Constraints:**

-   `1 <= n <= 1690`

# Solution 1: Dynamic Programming (Beat 100%)
```
public class Solution {
    public int nthUglyNumber(int n) {
        int[] dp = new int[n+1];
        dp[1] = 1;
        int index2 = 1, index3 = 1, index5 = 1;
        for(int i = 2; i <= n; i++){
            int tmp2 = 2*dp[index2];
            int tmp3 = 3*dp[index3];
            int tmp5 = 5*dp[index5];
            
            if(tmp2 <= tmp3 && tmp2 <= tmp5){
                index2++;
                dp[i] = tmp2;
                if(tmp2 == tmp3) index3++;
                if(tmp2 == tmp5) index5++;
            }else if(tmp3 <= tmp5 && tmp3 <= tmp2){
                index3++;
                dp[i] = tmp3;
                if(tmp3 == tmp5) index5++;
            }else{
                index5++;
                dp[i] = tmp5;
            }
        }
        
        return dp[n];
    }
    
    
}
```

Better coding skill:
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