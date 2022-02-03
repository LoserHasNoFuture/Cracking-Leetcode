# 390. Elimination Game
You have a list  `arr`  of all integers in the range  `[1, n]`  sorted in a strictly increasing order. Apply the following algorithm on  `arr`:

-   Starting from left to right, remove the first number and every other number afterward until you reach the end of the list.
-   Repeat the previous step again, but this time from right to left, remove the rightmost number and every other number from the remaining numbers.
-   Keep repeating the steps again, alternating left to right and right to left, until a single number remains.

Given the integer  `n`, return  _the last number that remains in_  `arr`.

**Example 1:**

**Input:** n = 9
**Output:** 6
**Explanation:**
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9]
arr = [2, 4, 6, 8]
arr = [2, 6]
arr = [6]

**Example 2:**

**Input:** n = 1
**Output:** 1

**Constraints:**

-   `1 <= n <= 109`

# Solution 1:
找规律, 计算每次remove之后头上剩下谁, 当然也要记录下当前每一步的间隔
```
class Solution {
    public int lastRemaining(int n) {
        int remaining = n, head = 1, step = 1;
        boolean left = true;
        
        while(remaining > 1){
            if(left || remaining % 2 == 1){
                head += step;
            }
            remaining = remaining/2;
            step = step * 2;
            left = !left;
        }
        
        return head;
    }
}
```