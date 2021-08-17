# 885. Spiral Matrix III 54 59
On a 2 dimensional grid with  `R`  rows and  `C`  columns, we start at  `(r0, c0)`  facing east.

Here, the north-west corner of the grid is at the first row and column, and the south-east corner of the grid is at the last row and column.

Now, we walk in a clockwise spiral shape to visit every position in this grid.

Whenever we would move outside the boundary of the grid, we continue our walk outside the grid (but may return to the grid boundary later.)

Eventually, we reach all  `R * C`  spaces of the grid.

Return a list of coordinates representing the positions of the grid in the order they were visited.

**Example 1:**

**Input:** R = 1, C = 4, r0 = 0, c0 = 0
**Output:** [[0,0],[0,1],[0,2],[0,3]]

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/24/example_1.png)

**Example 2:**

**Input:** R = 5, C = 6, r0 = 1, c0 = 4
**Output:** [[1,4],[1,5],[2,5],[2,4],[2,3],[1,3],[0,3],[0,4],[0,5],[3,5],[3,4],[3,3],[3,2],[2,2],[1,2],[0,2],[4,5],[4,4],[4,3],[4,2],[4,1],[3,1],[2,1],[1,1],[0,1],[4,0],[3,0],[2,0],[1,0],[0,0]]

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/24/example_2.png)

**Note:**

1.  `1 <= R <= 100`
2.  `1 <= C <= 100`
3.  `0 <= r0 < R`
4.  `0 <= c0 < C`

# Solution 1: Straight forward (Beat 87%)
```
class Solution {
    public int[][] spiralMatrixIII(int R, int C, int r0, int c0) {
        int[][] mat = new int[R*C][2];
        int len = 1, count = 0;
        int row = r0, col = c0;
        
        mat[count++] = new int[]{row, col};
        
        while(true){
            if(count == R*C) break;
            
            for(int i = col+1; i <= col + len && count < R*C; i++){
                if(i >= 0 && i < C && row >= 0 && row < R) mat[count++] = new int[]{row, i};
            }
            col = col + len; 
            
            for(int i = row+1; i <= row + len && count < R*C; i++){
                if(i >= 0 && i < R && col >= 0 && col < C) mat[count++] = new int[]{i, col};
            }
            row = row + len; 
            
            len++;
            
            for(int i = col-1; i >= col - len && count < R*C; i--){
                if(i >= 0 && i < C && row >= 0 && row < R) mat[count++] = new int[]{row, i};
            }
            col = col - len;
            
            for(int i = row-1; i >= row - len && count < R*C; i--){
                if(i >= 0 && i < R && col >= 0 && col < C) mat[count++] = new int[]{i, col};
            }
            row = row - len;
            
            len++;
        } 
        return mat;
    }
}
```


# Solution 2: Concise code (Beat 87%)
Refer from: [https://leetcode.com/problems/spiral-matrix-iii/discuss/158970/C%2B%2BJavaPython-112233-Steps](https://leetcode.com/problems/spiral-matrix-iii/discuss/158970/C%2B%2BJavaPython-112233-Steps)
```
class Solution {
        public int[][] spiralMatrixIII(int R, int C, int x, int y) {
        int[][] res = new int[R * C][2];
        int dx = 0, dy = 1, n = 0, tmp;
        for (int j = 0; j < R * C; ++n) {
            for (int i = 0; i < n / 2 + 1; ++i) {
                if (0 <= x && x < R && 0 <= y && y < C)
                    res[j++] = new int[] {x, y};
                x += dx;
                y += dy;
            }
            tmp = dx;
            dx = dy;
            dy = -tmp;
        }
        return res;
    }
}
```