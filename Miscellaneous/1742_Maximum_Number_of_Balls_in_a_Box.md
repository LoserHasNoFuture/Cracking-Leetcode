# 1742. Maximum Number of Balls in a Box
You are working in a ball factory where you have  `n`  balls numbered from  `lowLimit`  up to  `highLimit`  **inclusive**  (i.e.,  `n == highLimit - lowLimit + 1`), and an infinite number of boxes numbered from  `1`  to  `infinity`.

Your job at this factory is to put each ball in the box with a number equal to the sum of digits of the ball's number. For example, the ball number  `321`  will be put in the box number  `3 + 2 + 1 = 6`  and the ball number  `10`  will be put in the box number  `1 + 0 = 1`.

Given two integers  `lowLimit`  and  `highLimit`, return _the number of balls in the box with the most balls._

**Example 1:**

**Input:** lowLimit = 1, highLimit = 10
**Output:** 2
**Explanation:**
Box Number:  1 2 3 4 5 6 7 8 9 10 11 ...
Ball Count:  2 1 1 1 1 1 1 1 1 0  0  ...
Box 1 has the most number of balls with 2 balls.

**Example 2:**

**Input:** lowLimit = 5, highLimit = 15
**Output:** 2
**Explanation:**
Box Number:  1 2 3 4 5 6 7 8 9 10 11 ...
Ball Count:  1 1 1 1 2 2 1 1 1 0  0  ...
Boxes 5 and 6 have the most number of balls with 2 balls in each.

**Example 3:**

**Input:** lowLimit = 19, highLimit = 28
**Output:** 2
**Explanation:**
Box Number:  1 2 3 4 5 6 7 8 9 10 11 12 ...
Ball Count:  0 1 1 1 1 1 1 1 1 2  0  0  ...
Box 10 has the most number of balls with 2 balls.

**Constraints:**

-   `1 <= lowLimit <= highLimit <= 105`

# Solution 1: Brute Force (Beat 46%)
```
class Solution {
    public int countBalls(int lowLimit, int highLimit) {
        HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
    
        int max_count = 0;
        for(int i = lowLimit; i <= highLimit; i++){
            int val = 0, tmp = i;
            while(tmp != 0){
                val += tmp%10;
                tmp /= 10;
            }
            
            int cnt = map.getOrDefault(val,0)+1;
            if(cnt > max_count) max_count = cnt;
            
            map.put(val,cnt);
        }
        
        return max_count;
    }
}
```

# Solution 2: Improve Based on Solution 1 (Beat 100%)

the val only increase 1 when i % 10 != 0, so we only have to calculate it again when i%10 == 0;

```
class Solution {
    
    public int calculate_value(int tmp){
        int val = 0;
        while(tmp != 0){
            val += tmp%10;
            tmp /= 10;
        }
        
        return val;
    }
    
    public int countBalls(int lowLimit, int highLimit) {
        int[] vals = new int[highLimit+1];
        
        int val = calculate_value(lowLimit);
        vals[val]++;
        
        int max_count = 1;
        for(int i = lowLimit + 1; i <= highLimit; i++){
            if (i % 10 == 0) val = calculate_value(i);
            else val++;
            vals[val]++;
            max_count = Math.max(vals[val],max_count);
        }
        
        return max_count;
    }
}
```
