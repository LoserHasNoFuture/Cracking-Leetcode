#  351. Android Unlock Patterns (Not Finished)
Android devices have a special lock screen with a  `3 x 3`  grid of dots. Users can set an "unlock pattern" by connecting the dots in a specific sequence, forming a series of joined line segments where each segment's endpoints are two consecutive dots in the sequence. A sequence of  `k`  dots is a  **valid**  unlock pattern if both of the following are true:

-   All the dots in the sequence are  **distinct**.
-   If the line segment connecting two consecutive dots in the sequence passes through any other dot, the other dot  **must have previously appeared**  in the sequence. No jumps through non-selected dots are allowed.

Here are some example valid and invalid unlock patterns:

![](https://assets.leetcode.com/uploads/2018/10/12/android-unlock.png)

-   The 1st pattern  `[4,1,3,6]`  is invalid because the line connecting dots  `1`  and  `3`  pass through dot  `2`, but dot  `2`  did not previously appear in the sequence.
-   The 2nd pattern  `[4,1,9,2]`  is invalid because the line connecting dots  `1`  and  `9`  pass through dot  `5`, but dot  `5`  did not previously appear in the sequence.
-   The 3rd pattern  `[2,4,1,3,6]`  is valid because it follows the conditions. The line connecting dots  `1`  and  `3`  meets the condition because dot  `2`  previously appeared in the sequence.
-   The 4th pattern  `[6,5,4,1,9,2]`  is valid because it follows the conditions. The line connecting dots  `1`  and  `9`  meets the condition because dot  `5`  previously appeared in the sequence.

Given two integers  `m`  and  `n`, return  _the  **number of unique and valid unlock patterns**  of the Android grid lock screen that consist of  **at least**_ `m` _keys and  **at most**_ `n` _keys._

Two unlock patterns are considered  **unique**  if there is a dot in one sequence that is not in the other, or the order of the dots is different.

**Example 1:**

**Input:** m = 1, n = 1
**Output:** 9

**Example 2:**

**Input:** m = 1, n = 2
**Output:** 65

**Constraints:**

-   `1 <= m, n <= 9`


# Solution 1: BackTracking (Beat 50%)
```
class Solution {
    
    int[][] neighbors = {{-1,0},{1,0},{0,1},{0,-1},{1,1},{-1,-1},{-1,1},{1,-1},
                         {1,2},{1,-2},{-1,2},{-1,-2},{2,1},{2,-1},{-2,-1},{-2,1}};
    public int numberOfPatterns(int m, int n) {
        boolean[][] visited = new boolean[3][3];
        
        int res = 0;
        
        visited[0][0] = true;
        res += (dfs(0,0,visited,1,m,n)*4);
        visited[0][0] = false;
        
        visited[0][1] = true;
        res += (dfs(0,1,visited,1,m,n) * 4);
        visited[0][1] = false;
        
        visited[1][1] = true;
        res += dfs(1,1,visited,1,m,n);
        visited[1][1] = false;
        
        return res;
    }
    
    
    public int dfs(int x, int y, boolean[][] visited, int depth, int m, int n){
        int res = 0;
        
        if(depth >= m && depth <= n) res++;
        if(depth >= n) return res;
        
        for(int[] neigh: neighbors){
            int next_x = neigh[0] + x;
            int next_y = neigh[1] + y;
            
            if(next_x < 0 || next_y < 0 || next_x >= 3 || next_y >= 3) continue;
            
            while(visited[next_x][next_y]){
                next_x += neigh[0];
                next_y += neigh[1];
                if(next_x < 0 || next_y < 0 || next_x >= 3 || next_y >= 3) break;
            }
            
            if(next_x < 0 || next_y < 0 || next_x >= 3 || next_y >= 3) continue;
            
            visited[next_x][next_y] = true;
            res += dfs(next_x,next_y, visited, depth+1,m,n);
            visited[next_x][next_y] = false;
        }
        
        return res;
        
    }
}
```


# Solution 2: Ugly Solution (Beat 97%, NEED IMPROVE)
```
class Solution {
    
    int[][] neighbors = {{-1,0},{1,0},{0,1},{0,-1},{1,1},{-1,-1},{-1,1},{1,-1},
                         {1,2},{1,-2},{-1,2},{-1,-2},{2,1},{2,-1},{-2,-1},{-2,1}};
    public int numberOfPatterns(int m, int n) {
        boolean[][] visited = new boolean[3][3];
        
        int res = 0;
        
        res += start_from_edge(visited,m,n) * 4;
        
        res += start_from_01(visited,m,n) * 4;
        
        res += start_from_middle(visited,m,n);
        
        return res;
    }
    
    public int start_from_01(boolean[][] visited, int m, int n){
        int res = 0;
        visited[0][1] = true;
        if(m == 1) res++;
        
        visited[0][0] = true;
        res += dfs(0,0,visited,2,m,n) * 2;
        visited[0][0] = false;
        
        visited[1][1] = true;
        res += dfs(1,1,visited,2,m,n);
        visited[1][1] = false;
        
        visited[1][0] = true;
        res += dfs(1,0,visited,2,m,n) * 2;
        visited[1][0] = false;
        
        visited[2][0] = true;
        res += dfs(2,0,visited,2,m,n) * 2;
        visited[2][0] = false;
        
        
        visited[0][1] = false;
        return res;
    }
    
    public int start_from_edge(boolean[][] visited, int m, int n){
        int res = 0;
        visited[0][0] = true;
        if(m == 1) res++;
        
        visited[0][1] = true;
        res += dfs(0,1,visited,2,m,n) * 2;
        visited[0][1] = false;
        
        visited[1][1] = true;
        res += dfs(1,1,visited,2,m,n);
        visited[1][1] = false;
        
        visited[1][2] = true;
        res += dfs(1,2,visited,2,m,n) * 2;
        visited[1][2] = false;
        
        visited[0][0] = false;
        
        return res;
    }
    
    
    
    public int start_from_middle(boolean[][] visited, int m, int n){
        int res = 0;
        visited[1][1] = true;
        if(m == 1) res++;
        
        visited[0][0] = true;
        res += dfs(0,0,visited,2,m,n) * 4;
        visited[0][0] = false;
        
        visited[0][1] = true;
        res += dfs(0,1,visited,2,m,n) * 4;
        visited[0][1] = false;
        
        visited[1][1] = false;
        return res;
    }
    
    public int dfs(int x, int y, boolean[][] visited, int depth, int m, int n){
        int res = 0;
        
        if(depth >= m && depth <= n) res++;
        if(depth >= n) return res;
        
        
        for(int[] neigh: neighbors){
            int next_x = neigh[0] + x;
            int next_y = neigh[1] + y;
            
            if(next_x < 0 || next_y < 0 || next_x >= 3 || next_y >= 3) continue;
            
            while(visited[next_x][next_y]){
                next_x += neigh[0];
                next_y += neigh[1];
                if(next_x < 0 || next_y < 0 || next_x >= 3 || next_y >= 3) break;
            }
            
            if(next_x < 0 || next_y < 0 || next_x >= 3 || next_y >= 3) continue;
            
            visited[next_x][next_y] = true;
            res += dfs(next_x,next_y, visited, depth+1,m,n);
            visited[next_x][next_y] = false;
        }
        
        return res;
        
    }
}
```