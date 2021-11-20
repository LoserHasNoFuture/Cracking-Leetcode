# 367. Valid Perfect Square (Overflow)
Given a  **positive**  integer  _num_, write a function which returns True if  _num_  is a perfect square else False.

**Follow up:**  **Do not**  use any built-in library function such as  `sqrt`.

**Example 1:**

**Input:** num = 16
**Output:** true

**Example 2:**

**Input:** num = 14
**Output:** false

# Solution 1: Math
This is a math problem：  
1 = 1  
4 = 1 + 3  
9 = 1 + 3 + 5  
16 = 1 + 3 + 5 + 7  
25 = 1 + 3 + 5 + 7 + 9  
36 = 1 + 3 + 5 + 7 + 9 + 11  
....  
so 1+3+...+(2n-1) = (2n-1 + 1)_n/2 = nn

```
public boolean isPerfectSquare(int num) {
     int i = 1;
     while (num > 0) {
         num -= i;
         i += 2;
     }
     return num == 0;
 }
```



# Solution 2: Binary Search
Using long int to avoid overflow.
```
class Solution {
    public boolean isPerfectSquare(int num) {
        if(num <= 1) return true;
        long lnum = (long)num;
        long start = 2;
        long end = lnum/2;  
        
        while(start <= end){
            long mid = (start + end) >>> 1;
            if(mid*mid == lnum) return true;
            if(mid*mid > lnum) end = mid - 1;
            else start = mid + 1;
        }
        
        return false;
    }
}
```

Another trick to avoid overflow:
```
class Solution {
    public boolean isPerfectSquare(int num) {
        int i = 1, j = num;
        while(i <= j){
            int mid = i + (j-i)/2;
            // instead of calculating mid*mid, calculate num/mid
            int res = num/mid, tail = num%mid;
            if(tail == 0 && res == mid) return true;
            else if(res < mid){
                j = mid-1;
            } else i = mid+1;
        }
        return false;
    }
}
```

# Solution 3: Newton's Method
```
class Solution {
    public boolean isPerfectSquare(int num) {
        if(num <=1) return true;
        int temp = num/2;
        while(temp > num/temp){
            temp = (temp + num/temp)/2;
        }
        
        return (temp*temp == num);
    }
}
```