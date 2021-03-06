# 374. Guess Number Higher or Lower
### similar to 278, 744, 35

We are playing the Guess Game. The game is as follows:

I pick a number from  **1**  to  **_n_**. You have to guess which number I picked.

Every time you guess wrong, I'll tell you whether the number is higher or lower.

You call a pre-defined API  `guess(int num)`  which returns 3 possible results (`-1`,  `1`, or  `0`):

-1 : My number is lower
 1 : My number is higher
 0 : Congrats! You got it!

**Example :**

**Input:** n = 10, pick = 6
**Output:** 6

# Solution: Binary Search
```
public class Solution extends GuessGame {
    public int guessNumber(int n) {
        int start = 1;
        int end = n;
        
        while(start <= end){
            int mid = (start + end) >>> 1;
            int res = guess(mid);
            if(res == 0) return mid;
            if(res == -1) end = mid - 1;
            if(res == 1) start = mid + 1;
        }
        
        return 0;
    }
}
```