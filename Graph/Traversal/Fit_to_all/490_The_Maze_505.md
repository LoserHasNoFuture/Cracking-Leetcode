# 490. The Maze 505
There is a ball in a  `maze`  with empty spaces (represented as  `0`) and walls (represented as  `1`). The ball can go through the empty spaces by rolling  **up, down, left or right**, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.

Given the  `maze`, the ball's  `start`  position and the  `destination`, where  `start = [startrow, startcol]`  and  `destination = [destinationrow, destinationcol]`, return  `true`  if the ball can stop at the destination, otherwise return  `false`.

You may assume that  **the borders of the maze are all walls**  (see examples).

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/01/maze1.png)

**Input:** maze = [[0,0,1,0,0],[0,0,0,0,0],[0,0,0,1,0],[1,1,0,1,1],[0,0,0,0,0]], start = [0,4], destination = [4,4]
**Output:** true
**Explanation:** One possible way is : left -> down -> left -> down -> right -> down -> right.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/01/maze2.png)

**Input:** maze = [[0,0,1,0,0],[0,0,0,0,0],[0,0,0,1,0],[1,1,0,1,1],[0,0,0,0,0]], start = [0,4], destination = [3,2]
**Output:** false
**Explanation:** There is no way for the ball to stop at the destination. Notice that you can pass through the destination but you cannot stop there.

**Example 3:**

**Input:** maze = [[0,0,0,0,0],[1,1,0,0,1],[0,0,0,0,0],[0,1,0,0,1],[0,1,0,0,0]], start = [4,3], destination = [0,1]
**Output:** false

**Constraints:**

-   `1 <= maze.length, maze[i].length <= 100`
-   `maze[i][j]`  is  `0`  or  `1`.
-   `start.length == 2`
-   `destination.length == 2`
-   `0 <= startrow, destinationrow  <= maze.length`
-   `0 <= startcol, destinationcol  <= maze[i].length`
-   Both the ball and the destination exist on an empty space, and they will not be at the same position initially.
-   The maze contains  **at least 2 empty spaces**.


# Solution: BFS
```
class Solution {
    public boolean hasPath(int[][] maze, int[] start, int[] destination) {
        int row = maze.length;
        int col = maze[0].length;
        int[][] neighbors = new int[][]{{-1,0},{1,0},{0,-1},{0,1}};
        
        boolean[][] visited = new boolean[row][col];
        visited[start[0]][start[1]] = true;
        
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(start);
        
        while(!queue.isEmpty()){
            int[] cur = queue.poll();
            
            for(int[] neigh: neighbors){
                int count = 1;
                while(cur[0] + neigh[0]*count >= 0 && cur[0] + neigh[0]*count < row
                     && cur[1] + neigh[1]*count >= 0 && cur[1] + neigh[1]*count < col
                     && maze[cur[0] + neigh[0]*count][cur[1] + neigh[1]*count] != 1) {
                    count++;    
                }
                count--;
                if(!visited[cur[0] + neigh[0]*count][cur[1] + neigh[1]*count]){
                    if(cur[0] + neigh[0]*count == destination[0] 
                       && cur[1] + neigh[1]*count == destination[1]) return true;
                    
                    visited[cur[0] + neigh[0]*count][cur[1] + neigh[1]*count] = true;
                    queue.offer(new int[]{cur[0] + neigh[0]*count,cur[1] + neigh[1]*count});
                }
                
            }
            
        }
        
        return false;
    }
}
```

# Solution: DFS
```
class Solution {
    
    int[][] neighbors = new int[][]{{-1,0},{1,0},{0,-1},{0,1}};
    int row, col;
    boolean[][] visited;
    public boolean hasPath(int[][] maze, int[] start, int[] destination) {
        row = maze.length;
        col = maze[0].length;
        visited = new boolean[row][col];
        
        return dfs(maze,start,destination);
    }
    
    public boolean dfs(int[][] maze, int[] cur, int[] destination){
        
        for(int[] neigh: neighbors){
            int count = 1;
            while(cur[0] + neigh[0]*count >= 0 && cur[0] + neigh[0]*count < row
                 && cur[1] + neigh[1]*count >= 0 && cur[1] + neigh[1]*count < col
                 && maze[cur[0] + neigh[0]*count][cur[1] + neigh[1]*count] != 1) {
                if(cur[0] + neigh[0]*count == destination[0] && cur[1] + neigh[1]*count == destination[1]){
                    int x = destination[0] + neigh[0];
                    int y = destination[1] + neigh[1];
                    if(x < 0 || x >= row || y < 0 || y>= col || maze[x][y] == 1) return true;
                }
                count++;    
            }
            count--;
            if(!visited[cur[0] + neigh[0]*count][cur[1] + neigh[1]*count]){
                visited[cur[0] + neigh[0]*count][cur[1] + neigh[1]*count] = true;
                int[] start = new int[]{cur[0] + neigh[0]*count,cur[1] + neigh[1]*count};
                if(dfs(maze,start, destination)) return true;
            }

        }
    
        return false;
    }
}
```