# 1091. Shortest Path in Binary Matrix
In an N by N square grid, each cell is either empty (0) or blocked (1).

A _clear path from top-left to bottom-right_ has length  `k`  if and only if it is composed of cells  `C_1, C_2, ..., C_k` such that:

-   Adjacent cells  `C_i`  and  `C_{i+1}`  are connected 8-directionally (ie., they are different and share an edge or corner)
-   `C_1`  is at location  `(0, 0)`  (ie. has value  `grid[0][0]`)
-   `C_k` is at location  `(N-1, N-1)`  (ie. has value  `grid[N-1][N-1]`)
-   If  `C_i`  is located at `(r, c)`, then  `grid[r][c]`  is empty (ie. `grid[r][c] == 0`).

Return the length of the shortest such clear path from top-left to bottom-right. If such a path does not exist, return -1.

**Example 1:**

**Input:** [[0,1],[1,0]]
![](https://assets.leetcode.com/uploads/2019/08/04/example1_1.png) 
**Output:** 2
![](https://assets.leetcode.com/uploads/2019/08/04/example1_2.png)

**Example 2:**

**Input:** [[0,0,0],[1,1,0],[1,1,0]]
![](https://assets.leetcode.com/uploads/2019/08/04/example2_1.png) 
**Output:** 4
![](https://assets.leetcode.com/uploads/2019/08/04/example2_2.png)

**Note:**

1.  `1 <= grid.length == grid[0].length <= 100`
2.  `grid[r][c]`  is  `0`  or  `1`

# Solution 1: BFS (put distance in queue)
```
class Solution {
    
    class Node{
        int x;
        int y;
        int dis;
        
        Node(int x, int y, int dis){
            this.x = x;
            this.y = y;
            this.dis = dis;
        }
    }
    
    
    int[][] neighbors = new int[][]{{-1,0},{-1,-1},{-1,1},{1,0},{1,1},{1,-1},{0,1},{0,-1}};
    public int shortestPathBinaryMatrix(int[][] grid) {
        int n = grid.length;
        if(grid[0][0] == 1 || grid[n-1][n-1] == 1) return -1;
        if(n == 1) return 1;
        
        Queue<Node> queue = new LinkedList<Node>();
        queue.offer(new Node(0,0,1));
        grid[0][0] = 1;
        
        while(!queue.isEmpty()){
            Node cur = queue.poll();
            
            for(int[] neigh:neighbors){
                int next_x = neigh[0] + cur.x;
                int next_y = neigh[1] + cur.y;
                if(next_x < 0 || next_y < 0 || next_x >= n || next_y >= n || grid[next_x][next_y] == 1) continue;
                grid[next_x][next_y] = 1;
                if(next_x == n-1 && next_y == n-1) return cur.dis+1;
                queue.offer(new Node(next_x,next_y,cur.dis+1));
            }
        }
        
        
        return -1;
    }
}
```

# Solution 2: BFS (layer by layer)
```
class Solution {
    class Node{
        int x;
        int y;
        
        Node(int x, int y){
            this.x = x;
            this.y = y;
        }
    }
    
    
    int[][] neighbors = new int[][]{{-1,0},{-1,-1},{-1,1},{1,0},{1,1},{1,-1},{0,1},{0,-1}};
    public int shortestPathBinaryMatrix(int[][] grid) {
        int n = grid.length;
        if(grid[0][0] == 1 || grid[n-1][n-1] == 1) return -1;
        if(n == 1) return 1;
        
        Queue<Node> queue = new LinkedList<Node>();
        queue.offer(new Node(0,0));
        int dis = 1;
        grid[0][0] = 1;
        
        while(!queue.isEmpty()){
            int size = queue.size();
            
            for(int i = 0; i < size; i++){
                Node cur = queue.poll();
            
                for(int[] neigh:neighbors){
                    int next_x = neigh[0] + cur.x;
                    int next_y = neigh[1] + cur.y;
                    if(next_x < 0 || next_y < 0 || next_x >= n || next_y >= n || grid[next_x][next_y] == 1) continue;
                    grid[next_x][next_y] = 1;
                    if(next_x == n-1 && next_y == n-1) return dis+1;
                    queue.offer(new Node(next_x,next_y));
                }
            }
            dis++;
        }
        return -1;
    }
}
```