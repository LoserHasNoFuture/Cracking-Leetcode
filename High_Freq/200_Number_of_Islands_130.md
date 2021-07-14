# 200. Number of Islands 130
Given a 2d grid map of  `'1'`s (land) and  `'0'`s (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

**Input:** grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
**Output:** 1

**Example 2:**

**Input:** grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
**Output:** 3

# Solution 1: BFS
```
class Solution {
    
    public int numIslands(char[][] grid) {
        int counter = 0;
        
        for(int i = 0; i < grid.length; i++){
            for(int j = 0; j < grid[i].length; j++){
                if(grid[i][j] == '1'){
                    counter++;
                    BFS(grid, i, j);
                }
            }
        }
        
        return counter;
    }
    
    public void BFS(char[][] grid, int i, int j){
        Queue<int[]> q = new LinkedList<int[]>();
        q.offer(new int[]{i,j});
        grid[i][j] = '0';
        
        int[][] neighbors = new int[][]{
            {-1,0},{1,0},{0,-1},{0,1}
        };
        
        while(!q.isEmpty()){
            int[] pos = q.poll();   
            
            for(int[] n:neighbors){
                int rIndex = pos[0] - n[0];
                int cIndex = pos[1] - n[1];
                if(rIndex < 0 || rIndex >=  grid.length 
                   || cIndex < 0 || cIndex >= grid[0].length
                  || grid[rIndex][cIndex] == '0') continue;
                grid[rIndex][cIndex] = '0';
                q.offer(new int[]{rIndex, cIndex});
            }
            
        }   
    }
}
```

# Solution 2: DFS Recursion 
Can easily convert to DFS iteration
```
class Solution {
    
    public int numIslands(char[][] grid) {
        int counter = 0;
        
        for(int i = 0; i < grid.length; i++){
            for(int j = 0; j < grid[i].length; j++){
                if(grid[i][j] == '1'){
                    counter++;
                    DFS(grid, i, j);
                }
            }
        }
        
        return counter;
    }
    
    public void DFS(char[][] grid, int i, int j){
        if(i < 0 || i >= grid.length 
          || j < 0 || j >= grid[0].length 
           || grid[i][j] == '0') return;
        grid[i][j] = '0';
        DFS(grid,i-1,j);
        DFS(grid,i+1,j);
        DFS(grid,i,j-1);
        DFS(grid,i,j+1);
    }
    
}
```

# Solution 3: Union Find
Copy from discuss section:
```
class Solution {
    int[][] distance = {{1,0},{-1,0},{0,1},{0,-1}};
    public int numIslands(char[][] grid) {  
        if (grid == null || grid.length == 0 || grid[0].length == 0)  {
            return 0;  
        }
        UnionFind uf = new UnionFind(grid);  
        int rows = grid.length;  
        int cols = grid[0].length;  
        for (int i = 0; i < rows; i++) {  
            for (int j = 0; j < cols; j++) {  
                if (grid[i][j] == '1') {  
                    for (int[] d : distance) {
                        int x = i + d[0];
                        int y = j + d[1];
                        if (x >= 0 && x < rows && y >= 0 && y < cols && grid[x][y] == '1') {  
                            int id1 = i*cols+j;
                            int id2 = x*cols+y;
                            uf.union(id1, id2);  
                        }  
                    }  
                }  
            }  
        }  
        return uf.count;  
    }
    
    class UnionFind {
        int[] father;  
        int m, n;
        int count = 0;
        UnionFind(char[][] grid) {  
            m = grid.length;  
            n = grid[0].length;  
            father = new int[m*n];  
            for (int i = 0; i < m; i++) {  
                for (int j = 0; j < n; j++) {  
                    if (grid[i][j] == '1') {
                        int id = i * n + j;
                        father[id] = id;
                        count++;
                    }
                }  
            }  
        }
        public void union(int node1, int node2) {  
            int find1 = find(node1);
            int find2 = find(node2);
            if(find1 != find2) {
                father[find1] = find2;
                count--;
            }
        }
        public int find (int node) {  
            if (father[node] == node) {  
                return node;
            }
            father[node] = find(father[node]);  
            return father[node];
        }
    }
    
}
```