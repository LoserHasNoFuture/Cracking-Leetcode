# 441. Arranging Coins （Overflow）
You have a total of  _n_  coins that you want to form in a staircase shape, where every  _k_-th row must have exactly  _k_  coins.

Given  _n_, find the total number of  **full**  staircase rows that can be formed.

_n_  is a non-negative integer and fits within the range of a 32-bit signed integer.

**Example 1:**

n = 5

The coins can form the following rows:
¤
¤ ¤
¤ ¤

Because the 3rd row is incomplete, we return 2.

**Example 2:**

n = 8

The coins can form the following rows:
¤
¤ ¤
¤ ¤ ¤
¤ ¤

Because the 4th row is incomplete, we return 3.

# Solution 1: Brute Force
```
public class Solution {
    public int arrangeCoins(int n) {
        int i=0;
        while(n > 0){
            i+=1;
            n-=i;
        }
        return n==0 ? i : i-1;  
    }
}
```

# Solution 2: Math
Using long int to avoid overflow
```
public class Solution {
    public int arrangeCoins(int n) {
        return (int)((-1 + Math.sqrt(1 + 8 * (long)n)) / 2);
    }
}
```
# Solution 3: Binary Search
Using long int to avoid overflow
```
class Solution {
    public int arrangeCoins(int n) {
        if (n <= 1) return n;
        long nlong = (long)n;
        long start = 0;
        long end = nlong;
        
        while(start < end){
            long mid = (end + start + 1) >>> 1;
            long res = mid * (mid + 1);
            if(res == 2 * nlong) return (int)mid;
            if(res > 2 * nlong) end = mid - 1;
            else start = mid;
        }
        
        return (int)(start);
    }
}
```
