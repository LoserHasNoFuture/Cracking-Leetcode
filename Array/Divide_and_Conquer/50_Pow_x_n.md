# 50. Pow(x, n)
Implement  [pow(x, n)](http://www.cplusplus.com/reference/valarray/pow/), which calculates  `x`  raised to the power  `n`  (i.e.,  `xn`).

**Example 1:**

**Input:** x = 2.00000, n = 10
**Output:** 1024.00000

**Example 2:**

**Input:** x = 2.10000, n = 3
**Output:** 9.26100

**Example 3:**

**Input:** x = 2.00000, n = -2
**Output:** 0.25000
**Explanation:** 2-2 = 1/22 = 1/4 = 0.25

**Constraints:**

-   `-100.0 < x < 100.0`
-   `-231 <= n <= 231-1`
-   `-104  <= xn  <= 104`


# Solution : Iterative
### 注意Integer.MIN_VALUE需要特殊处理
```
class Solution {
    public double myPow(double x, int n) {
        if(n == 0) return 1.0;
        if(x == 0) return x;
        if(n == Integer.MIN_VALUE){ // Math.abs(Integer.MIN_VALUE) - Math.abs(Integer.MAX_VALUE) > 0
            n = n/2;
            x = x*x;
        }
        if(n < 0){
            n = -n;
            x = 1/x;
        }
        double res = 1.0;
        while(n>0){
            if((n&1) != 0) res *=x;
            x *= x;
            n = n >>> 1;
        }
        
        return res;
    }
}
```

# Solution 2: Recursive
```
class Solution {
    public double myPow(double x, int n) {
        if(n == 0) return 1;
        if(x == 1) return x;
        if(n < 0) {
            if(n == Integer.MIN_VALUE) return 1/myPow(x,-(n+1))/x;
            return 1/myPow(x,-n);
        }
        
        double half = myPow(x,n/2);
        if(n%2 == 0) return half*half;
        else return half*half*x;
    }  
    
    
}
```