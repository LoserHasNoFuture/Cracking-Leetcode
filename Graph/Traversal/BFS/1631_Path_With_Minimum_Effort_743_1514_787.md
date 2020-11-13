# 1631. Path With Minimum Effort 743 1514 787
You are a hiker preparing for an upcoming hike. You are given  `heights`, a 2D array of size  `rows x columns`, where  `heights[row][col]`  represents the height of cell  `(row, col)`. You are situated in the top-left cell,  `(0, 0)`, and you hope to travel to the bottom-right cell,  `(rows-1, columns-1)`  (i.e., **0-indexed**). You can move  **up**,  **down**,  **left**, or  **right**, and you wish to find a route that requires the minimum  **effort**.

A route's  **effort**  is the  **maximum absolute difference**  in heights between two consecutive cells of the route.

Return  _the minimum  **effort**  required to travel from the top-left cell to the bottom-right cell._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/04/ex1.png)

**Input:** heights = [[1,2,2],[3,8,2],[5,3,5]]
**Output:** 2
**Explanation:** The route of [1,3,5,3,5] has a maximum absolute difference of 2 in consecutive cells.
This is better than the route of [1,2,2,2,5], where the maximum absolute difference is 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/04/ex2.png)

**Input:** heights = [[1,2,3],[3,8,4],[5,3,5]]
**Output:** 1
**Explanation:** The route of [1,2,3,4,5] has a maximum absolute difference of 1 in consecutive cells, which is better than route [1,3,5,3,5].

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/10/04/ex3.png)

**Input:** heights = [[1,2,1,1,1],[1,2,1,2,1],[1,2,1,2,1],[1,2,1,2,1],[1,1,1,2,1]]
**Output:** 0
**Explanation:** This route does not require any effort.

**Constraints:**

-   `rows == heights.length`
-   `columns == heights[i].length`
-   `1 <= rows, columns <= 100`
-   `1 <= heights[i][j] <= 106`

# Solution 1: BFS + Priority Queue = Dijstra
```
class Node{
    int x;
    int y;
    int effort;
    
    Node(int _x, int _y, int _effort){
        this.x = _x;
        this.y = _y;
        this.effort = _effort;
    }
}

class Solution {
    public int minimumEffortPath(int[][] heights) {
        int row = heights.length;
        int col = heights[0].length;
        if(row == 0 && col == 0) return 0;
        
        int[][] neighbors = new int[][]{{-1,0},{1,0},{0,-1},{0,1}};
        PriorityQueue<Node> queue = new PriorityQueue<Node>((a,b)->a.effort-b.effort);
        queue.offer(new Node(0,0,0));
        
        while(!queue.isEmpty()){
            Node cur = queue.poll();
            if(heights[cur.x][cur.y] < 0) continue;
            if(cur.x == row - 1 && cur.y == col -1) return cur.effort;
            heights[cur.x][cur.y] = -heights[cur.x][cur.y];
            
            for(int[] neigh:neighbors){
                int x = cur.x + neigh[0];
                int y = cur.y + neigh[1];
                if(x >= 0 && x < row && y >= 0 && y < col && heights[x][y] > 0){
                    int effort = Math.max(cur.effort, Math.abs(heights[x][y]+ heights[cur.x][cur.y]));
                    queue.offer(new Node(x,y,effort));
                }
            }
            
        }
        
        return -1;
    }
}
```