# 1391. Check if There is a Valid Path in a Grid
Given a _m_ x _n_  `grid`. Each cell of the `grid` represents a street. The street of `grid[i][j]` can be:

-   **1**  which means a street connecting the left cell and the right cell.
-   **2**  which means a street connecting the upper cell and the lower cell.
-   **3** which means a street connecting the left cell and the lower cell.
-   **4**  which means a street connecting the right cell and the lower cell.
-   **5**  which means a street connecting the left cell and the upper cell.
-   **6**  which means a street connecting the right cell and the upper cell.

![](https://assets.leetcode.com/uploads/2020/03/05/main.png)

You will initially start at the street of the upper-left cell  `(0,0)`. A valid path in the grid is a path which starts from the upper left cell  `(0,0)`  and ends at the bottom-right cell  `(m - 1, n - 1)`.  **The path should only follow the streets**.

**Notice**  that you are  **not allowed**  to change any street.

Return  _true_ if there is a valid path in the grid or  _false_  otherwise.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/03/05/e1.png)

**Input:** grid = [[2,4,3],[6,5,2]]
**Output:** true
**Explanation:** As shown you can start at cell (0, 0) and visit all the cells of the grid to reach (m - 1, n - 1).

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/03/05/e2.png)

**Input:** grid = [[1,2,1],[1,2,1]]
**Output:** false
**Explanation:** As shown you the street at cell (0, 0) is not connected with any street of any other cell and you will get stuck at cell (0, 0)

**Example 3:**

**Input:** grid = [[1,1,2]]
**Output:** false
**Explanation:** You will get stuck at cell (0, 1) and you cannot reach cell (0, 2).

**Example 4:**

**Input:** grid = [[1,1,1,1,1,1,3]]
**Output:** true

**Example 5:**

**Input:** grid = [[2],[2],[2],[2],[2],[2],[6]]
**Output:** true

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `1 <= m, n <= 300`
-   `1 <= grid[i][j] <= 6`

# Solution: BFS
```
class Point{
    int x;
    int y;
    Point(int _x, int _y){
        this.x = _x;
        this.y = _y;
    }
}

class Solution {
    int[][][] dirs = {
                {{0, -1}, {0, 1}},
                {{-1, 0}, {1, 0}},
                {{0, -1}, {1, 0}},
                {{0, 1}, {1, 0}},
                {{0, -1}, {-1, 0}},
                {{0, 1}, {-1, 0}}
    };
    
    public boolean hasValidPath(int[][] grid) {
        int row = grid.length;
        int col = grid[0].length;
        
        boolean[][] visited  = new boolean[row][col];
        Queue<Point> queue = new LinkedList<Point>();
        queue.offer(new Point(0,0));
        
        while(!queue.isEmpty()){
            Point cur = queue.poll();
            if(cur.x == row - 1 && cur.y == col - 1) return true;
            
            visited[cur.x][cur.y] = true;
            int street = grid[cur.x][cur.y];
            
            for(int[] dir : dirs[street-1]){
                Point next = new Point(cur.x+dir[0],cur.y+dir[1]);
                if(next.x < 0 || next.x >= row) continue;
                if(next.y < 0 || next.y >= col) continue;
                if(visited[next.x][next.y]) continue;
                
                if(can_next_back_to_cur(cur,next,grid[next.x][next.y])){
                    queue.offer(next);
                }
            }
            
        }
        
        return false;
    }
    
    public boolean can_next_back_to_cur(Point cur, Point next, int street){
        for(int[] dir : dirs[street-1]){
            if(next.x + dir[0] == cur.x && next.y + dir[1] == cur.y) return true;
        }
        return false;
    }
}
```