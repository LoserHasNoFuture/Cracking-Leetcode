# 542. 01 Matrix
Given a matrix consists of 0 and 1, find the distance of the nearest 0 for each cell.

The distance between two adjacent cells is 1.

**Example 1:**

**Input:**
[[0,0,0],
 [0,1,0],
 [0,0,0]]

**Output:**
[[0,0,0],
 [0,1,0],
 [0,0,0]]

**Example 2:**

**Input:**
[[0,0,0],
 [0,1,0],
 [1,1,1]]

**Output:**
[[0,0,0],
 [0,1,0],
 [1,2,1]]

**Note:**

1.  The number of elements of the given matrix will not exceed 10,000.
2.  There are at least one 0 in the given matrix.
3.  The cells are adjacent in only four directions: up, down, left and right.

# Solution: BFS
```
class Solution {
    int[][] neighbors = new int[][]{{-1,0},{1,0},{0,-1},{0,1}};
    public int[][] updateMatrix(int[][] matrix) {
        int row = matrix.length;
        int col = matrix[0].length;
        
        Queue<int[]> queue = new LinkedList<int[]>();
        
        for(int i = 0; i < row; i++){
            for(int j = 0; j < col; j++){
                if(matrix[i][j] == 0){
                    queue.offer(new int[]{i,j});
                }else matrix[i][j] = Integer.MAX_VALUE;
            }
        }
        
        while(!queue.isEmpty()){
            int[] cur = queue.poll();
            
            for(int[] neigh:neighbors){
                int _x = cur[0] + neigh[0];
                int _y = cur[1] + neigh[1];
                if(_x < 0 || _x >= row || _y < 0 || _y >= col 
                   || matrix[_x][_y] <= matrix[cur[0]][cur[1]]+1) continue;
                matrix[_x][_y] = matrix[cur[0]][cur[1]]+1;
                queue.offer(new int[]{_x,_y});
            }
        }
         
        return matrix;
    }
}
```

# Solution 2: DP
Refer From: [https://leetcode.com/problems/01-matrix/discuss/101051/Simple-Java-solution-beat-99-(use-DP)](https://leetcode.com/problems/01-matrix/discuss/101051/Simple-Java-solution-beat-99-(use-DP))
```
class Solution {
    public int[][] updateMatrix(int[][] matrix) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return matrix;
        }
        int[][] dis = new int[matrix.length][matrix[0].length];
        int range = matrix.length * matrix[0].length;

	// start from the beginning, update all distances
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                if (matrix[i][j] == 0) {
                    dis[i][j] = 0;
                } else {
                    int upCell = (i > 0) ? dis[i - 1][j] : range;
                    int leftCell = (j > 0) ? dis[i][j - 1] : range;
                    dis[i][j] = Math.min(upCell, leftCell) + 1;
                }
            }
        }
	// start from the end, re-update all distances
        for (int i = matrix.length - 1; i >= 0; i--) {
            for (int j = matrix[0].length - 1; j >= 0; j--) {
                if (matrix[i][j] == 0) {
                    dis[i][j] = 0;
                } else {
                    int downCell = (i < matrix.length - 1) ? dis[i + 1][j] : range;
                    int rightCell = (j < matrix[0].length - 1) ? dis[i][j + 1] : range;
                    dis[i][j] = Math.min(Math.min(downCell, rightCell) + 1, dis[i][j]);
                }
            }
        }

        return dis;
    }
}
```

