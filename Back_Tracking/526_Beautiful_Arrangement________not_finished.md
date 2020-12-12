# 526. Beautiful Arrangement  (Not Finished)
Suppose you have  **N**  integers from 1 to N. We define a beautiful arrangement as an array that is constructed by these  **N**  numbers successfully if one of the following is true for the ith  position (1 <= i <= N) in this array:

1.  The number at the ith  position is divisible by  **i**.
2.  **i**  is divisible by the number at the ith  position.

Now given N, how many beautiful arrangements can you construct?

**Example 1:**

**Input:** 2
**Output:** 2
**Explanation:** 

The first beautiful arrangement is [1, 2]:

Number at the 1st position (i=1) is 1, and 1 is divisible by i (i=1).

Number at the 2nd position (i=2) is 2, and 2 is divisible by i (i=2).

The second beautiful arrangement is [2, 1]:

Number at the 1st position (i=1) is 2, and 2 is divisible by i (i=1).

Number at the 2nd position (i=2) is 1, and i (i=2) is divisible by 1.

**Note:**

1.  **N**  is a positive integer and will not exceed 15.

# Solution 1: BackTracking (Beat 86%)
```
class Solution {
    
    boolean[][] links;
    boolean[] visited;
    int res = 0;
    public int countArrangement(int N) {
        visited = new boolean[N+1];
        links = new boolean[N+1][N+1]; 
        
        <!-- to save mode operations in backtracking-->
        for(int i = 1; i <= N; i++){
            for(int j = i; j <= N; j++){
                if(i%j == 0 || j%i == 0) {
                    links[i][j] = true;
                    links[j][i] = true;
                }
            }
        }
        
        for(int i = 1; i <= N; i++){
            visited[i] = true;
            dfs(i, 1, N);
            visited[i] = false;
        }
        
        return res;
    }
    
    public void dfs(int cur, int len, int N){
        if(len == N) {
            res++;
            return;
        }
        
        for(int j = 1; j <= N; j++){
            if(links[len+1][j] && !visited[j]){
                visited[j] = true;
                dfs(j,len + 1, N);
                visited[j] = false;
            }
        }    
            
    }
    
}
```

# Solution 2:  I could not understand (Beat 99%)
Try this Later
Ref: [https://leetcode.com/problems/beautiful-arrangement/discuss/99711/Java-6ms-beats-98-back-tracking-(swap)-starting-from-the-back](https://leetcode.com/problems/beautiful-arrangement/discuss/99711/Java-6ms-beats-98-back-tracking-(swap)-starting-from-the-back)
