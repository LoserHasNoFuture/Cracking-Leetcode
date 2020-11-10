# 505. The Maze II 490
There is a  **ball**  in a maze with empty spaces and walls. The ball can go through empty spaces by rolling  **up**,  **down**,  **left**  or  **right**, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.

Given the ball's  **start position**, the  **destination**  and the  **maze**, find the shortest distance for the ball to stop at the destination. The distance is defined by the number of  **empty spaces**  traveled by the ball from the start position (excluded) to the destination (included). If the ball cannot stop at the destination, return -1.

The maze is represented by a binary 2D array. 1 means the wall and 0 means the empty space. You may assume that the borders of the maze are all walls. The start and destination coordinates are represented by row and column indexes.

**Example 1:**

**Input 1:** a maze represented by a 2D array

0 0 1 0 0
0 0 0 0 0
0 0 0 1 0
1 1 0 1 1
0 0 0 0 0

**Input 2:** start coordinate (rowStart, colStart) = (0, 4)
**Input 3:** destination coordinate (rowDest, colDest) = (4, 4)

**Output:** 12

**Explanation:** One shortest way is : left -> down -> left -> down -> right -> down -> right.
             The total distance is 1 + 1 + 3 + 1 + 2 + 2 + 2 = 12.
![](https://assets.leetcode.com/uploads/2018/10/12/maze_1_example_1.png)

**Example 2:**

**Input 1:** a maze represented by a 2D array

0 0 1 0 0
0 0 0 0 0
0 0 0 1 0
1 1 0 1 1
0 0 0 0 0

**Input 2:** start coordinate (rowStart, colStart) = (0, 4)
**Input 3:** destination coordinate (rowDest, colDest) = (3, 2)

**Output:** -1

**Explanation:** There is no way for the ball to stop at the destination.
![](https://assets.leetcode.com/uploads/2018/10/13/maze_1_example_2.png)

**Note:**

1.  There is only one ball and one destination in the maze.
2.  Both the ball and the destination exist on an empty space, and they will not be at the same position initially.
3.  The given maze does not contain border (like the red rectangle in the example pictures), but you could assume the border of the maze are all walls.
4.  The maze contains at least 2 empty spaces, and both the width and height of the maze won't exceed 100.

# Dijistra: BFS+PriorityQueue
```
class Solution {
    
    public int shortestDistance(int[][] maze, int[] start, int[] destination) {
        int row = maze.length;
        int col = maze[0].length;
        
        int[][] neighbors = new int[][]{{-1,0},{1,0},{0,-1},{0,1}};
        int[][] steps = new int[row][col];
        
        for(int i = 0; i < row*col; i++) steps[i/col][i%col] = Integer.MAX_VALUE;
            
        PriorityQueue<int[]> queue = new PriorityQueue<>((a,b)->a[2]-b[2]);
        queue.offer(new int[]{start[0],start[1],0});
        
        while(!queue.isEmpty()){
            int[] cur = queue.poll();
            if(destination[0] == cur[0] && destination[1] == cur[1]) return cur[2];
            if(cur[2] >= steps[cur[0]][cur[1]]) continue; 
            steps[cur[0]][cur[1]] = cur[2];
            
            for(int[] neigh: neighbors){
                int count = 1;
                while(cur[0] + neigh[0]*count >= 0 && cur[0] + neigh[0]*count < row
                     && cur[1] + neigh[1]*count >= 0 && cur[1] + neigh[1]*count < col
                     && maze[cur[0] + neigh[0]*count][cur[1] + neigh[1]*count] != 1) {
                    count++;    
                }
                count--;
                if(steps[cur[0] + neigh[0]*count][cur[1] + neigh[1]*count] > cur[2]+count){
                    queue.offer(new int[]{cur[0] + neigh[0]*count,cur[1] + neigh[1]*count,cur[2]+count});
                }   
            }    
        }
        
        
        return -1;
    }
    
}
```