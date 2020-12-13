# 357. Count Numbers with Unique Digits
Given a  **non-negative**  integer n, count all numbers with unique digits, x, where 0 ≤ x < 10n.

**Example:**

**Input:** 2
**Output:** 91 
**Explanation:** The answer should be the total numbers in the range of 0 ≤ x < 100, 
             excluding `11,22,33,44,55,66,77,88,99`

**Constraints:**

-   `0 <= n <= 8`

# Solution 1: BackTracking (Beat 100%)
```
class Solution {
    
    boolean[] visited = new boolean[10];
    public int countNumbersWithUniqueDigits(int n) {
        return dfs(n,0);
    }
    
    public int dfs(int len, int depth){
        
        if(depth == len) return 1;
        
        int sum = 1;
        int tmp = 0;
        for(int i = depth == 0? 1:0; i <= 9; i++){
            if(!visited[i]){
                if(tmp == 0){
                    visited[i] = true;
                    tmp = dfs(len,depth+1);    
                    visited[i] = false;
                }
                sum += tmp;
            }
        }
        
        return sum;
    }
}
```