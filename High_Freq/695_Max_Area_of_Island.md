# 695. Max Area of Island
You are given an  `m x n`  binary matrix  `grid`. An island is a group of  `1`'s (representing land) connected  **4-directionally**  (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The  **area**  of an island is the number of cells with a value  `1`  in the island.

Return  _the maximum  **area**  of an island in_ `grid`. If there is no island, return  `0`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/01/maxarea1-grid.jpg)

**Input:** grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
**Output:** 6
**Explanation:** The answer is not 11, because the island must be connected 4-directionally.

**Example 2:**

**Input:** grid = [[0,0,0,0,0,0,0,0]]
**Output:** 0

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `1 <= m, n <= 50`
-   `grid[i][j]`  is either  `0`  or  `1`.


# Solution 1: DFS / BFS
```
class Solution {
    int m,n;
    int[][] neighbors = new int[][]{{-1,0},{1,0},{0,1},{0,-1}};
    public int maxAreaOfIsland(int[][] grid) {
        int res = 0;
        m = grid.length; n = grid[0].length;
        
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(grid[i][j] == 1){
                    // res = Math.max(res, dfs(grid,i,j));
                    res = Math.max(res, bfs(grid,i,j));
                }
            }
        }
        
        return res;
    }
    
    public int bfs(int[][] grid, int _x, int _y){
        Queue<int[]> queue = new LinkedList<int[]>();
        int res = 0;
        queue.offer(new int[]{_x,_y});
        grid[_x][_y] = 0;
        
        while(!queue.isEmpty()){
            res++;
            int[] cur = queue.poll();
            
            for(int[] neigh:neighbors){
                int x = cur[0] + neigh[0];
                int y = cur[1] + neigh[1];
                
                if(x < 0 || y < 0 || x >= m || y >= n || grid[x][y] == 0) continue;
                grid[x][y] = 0;
                queue.offer(new int[]{x,y});
            }
        }
        
        return res;
    }
    
    public int dfs(int[][] grid, int x, int y){
        if(x < 0 || y < 0 || x >= m || y >= n || grid[x][y] == 0) return 0;
        int res = 1;
        grid[x][y] = 0;
        
        res += dfs(grid,x-1,y);
        res += dfs(grid,x+1,y);
        res += dfs(grid,x,y-1);
        res += dfs(grid,x,y+1);
        
        return res;
    }
}
```

# Solution 2: Union Find
```
class Solution {
    public List<Integer> numIslands2(int m, int n, int[][] positions) {
        char[][] grid = new char[m][n];
        int[] id = new int[m*n], sz = new int[m*n];
        
        for(int i = 0; i < m*n; i++) id[i] = i;
        List<Integer> res = new ArrayList<Integer>();
        int cnt = 0;
        
        for(int[] pos: positions){
            int i = pos[0], j = pos[1];
            if(grid[i][j] != '1'){
                cnt++;
                grid[i][j] = '1';
                if(i > 0 && grid[i-1][j] == '1') cnt += union(id,sz,i*n+j,(i-1)*n+j);
                if(i < m - 1 && grid[i+1][j] == '1') cnt += union(id,sz,i*n+j,(i+1)*n+j);
                if(j > 0 && grid[i][j-1] == '1') cnt += union(id,sz,i*n+j,i*n+j-1);
                if(j < n - 1 && grid[i][j+1] == '1') cnt += union(id,sz,i*n+j,i*n+j+1);
            }
            res.add(cnt);
        }
        
        return res;
    }
    
    public int union(int[] id, int[] sz, int x, int y){
        int root1 = find_root(id,x);
        int root2 = find_root(id,y);
        if(root1 == root2) return 0;
        
        if(sz[root1] > sz[root2]) id[root2] = root1;
        else{
            id[root1] = root2;
            sz[root2] = Math.max(sz[root2],sz[root1] + 1);
        }
        
        return -1;
    }
    
    public int find_root(int[] id, int p){
        while(id[p] != p){
            id[p] = id[id[p]];
            p = id[p];
        }
        return p;
    }
}
```