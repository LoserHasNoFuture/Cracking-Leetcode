# 576. Out of Boundary Paths
There is an  `m x n`  grid with a ball. The ball is initially at the position  `[startRow, startColumn]`. You are allowed to move the ball to one of the four adjacent cells in the grid (possibly out of the grid crossing the grid boundary). You can apply  **at most**  `maxMove`  moves to the ball.

Given the five integers  `m`,  `n`,  `maxMove`,  `startRow`,  `startColumn`, return the number of paths to move the ball out of the grid boundary. Since the answer can be very large, return it  **modulo**  `109  + 7`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_1.png)

**Input:** m = 2, n = 2, maxMove = 2, startRow = 0, startColumn = 0
**Output:** 6

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_2.png)

**Input:** m = 1, n = 3, maxMove = 3, startRow = 0, startColumn = 1
**Output:** 12

**Constraints:**

-   `1 <= m, n <= 50`
-   `0 <= maxMove <= 50`
-   `0 <= startRow < m`
-   `0 <= startColumn < n`

# Solution 1: BFS (Beat 10%)
```
class Solution {
    int[][] neighbors = new int[][]{{-1,0},{1,0},{0,1},{0,-1}};
    int mod = 1000000007;
    
    public int findPaths(int m, int n, int maxMove, int startRow, int startColumn) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        map.put(startRow*n + startColumn,1);
        
        int res = 0;
        int steps = 0;
        while(!map.isEmpty()){
            if(steps >= maxMove) break;
            HashMap<Integer, Integer> tmp = new HashMap<Integer, Integer>();
            
            for(int cur: map.keySet()){
                int x = cur/n;
                int y = cur%n;
                int cnt = map.get(cur);
                
                for(int[] neigh: neighbors){
                    int _x = x + neigh[0];
                    int _y = y + neigh[1];
                    if(_x < 0 || _y < 0 || _x >= m || _y >= n) {
                        res = ((res%mod) + (cnt%mod))%mod;
                        continue;
                    }
                    int _cnt = ((tmp.getOrDefault(_x*n + _y,0)%mod) + (cnt%mod))%mod;
                    tmp.put(_x*n + _y,_cnt);
                }
            }
            map = tmp;
            steps++;
        }
        
        return res;
    }
}
```

# Solution 2: DFS with Memorization (Beat 50%)
```
class Solution {
    int[][] neighbors = new int[][]{{-1,0},{1,0},{0,1},{0,-1}};
    int mod = 1000000007;
    int[][][] dp;
    public int findPaths(int m, int n, int maxMove, int startRow, int startColumn) {
        if(maxMove == 0) return 0;
        dp = new int[m][n][maxMove + 1];
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                for(int k = 0; k <= maxMove; k++){
                    dp[i][j][k] = -1;
                }
            }
        }
        
        dfs(maxMove,startRow,startColumn,m,n);
        
        return dp[startRow][startColumn][maxMove];
    }
    
    public int dfs(int steps, int x, int y, int m, int n){
        // System.out.println(x+ ", " + y + ", " + steps);
        if(x < 0 || y < 0 || x >= m || y >= n) return 1;
        if(steps == 0) return 0;
        if(dp[x][y][steps] != -1) return dp[x][y][steps];
        
        
        int res = 0;
        for(int[] neigh: neighbors){
            int _x = x + neigh[0];
            int _y = y + neigh[1];
            res = (res%mod + dfs(steps-1, _x, _y, m, n)%mod)%mod;
        }
        
        dp[x][y][steps] = res;
        return res;
    }
}
```

Ref: [https://leetcode.com/problems/out-of-boundary-paths/discuss/102967/Java-Solution-DP-with-space-compression](https://leetcode.com/problems/out-of-boundary-paths/discuss/102967/Java-Solution-DP-with-space-compression)
2D Memory.  
```
public class Solution {
    public int findPaths(int m, int n, int N, int i, int j) {
        if (N <= 0) return 0;
        
        final int MOD = 1000000007;
        int[][] count = new int[m][n];
        count[i][j] = 1;
        int result = 0;
        
        int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        
        for (int step = 0; step < N; step++) {
            int[][] temp = new int[m][n];
            for (int r = 0; r < m; r++) {
                for (int c = 0; c < n; c++) {
                    for (int[] d : dirs) {
                        int nr = r + d[0];
                        int nc = c + d[1];
                        if (nr < 0 || nr >= m || nc < 0 || nc >= n) {
                            result = (result + count[r][c]) % MOD;
                        }
                        else {
                            temp[nr][nc] = (temp[nr][nc] + count[r][c]) % MOD;
                        }
                    }
                }
            }
            count = temp;
        }
        
        return result;
    }
}
```
