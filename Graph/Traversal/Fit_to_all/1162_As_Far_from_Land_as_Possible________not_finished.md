# 1162. As Far from Land as Possible
Given an N x N  `grid` containing only values  `0`  and  `1`, where `0`  represents water and  `1`  represents land, find a water cell such that its distance to the nearest land cell is maximized and return the distance.

The distance used in this problem is the  _Manhattan distance_: the distance between two cells  `(x0, y0)`  and  `(x1, y1)`  is  `|x0 - x1| + |y0 - y1|`.

If no land or water exists in the grid, return  `-1`.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2019/05/03/1336_ex1.JPG)**

**Input:** [[1,0,1],[0,0,0],[1,0,1]]
**Output:** 2
**Explanation:** 
The cell (1, 1) is as far as possible from all the land with distance 2.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2019/05/03/1336_ex2.JPG)**

**Input:** [[1,0,0],[0,0,0],[0,0,0]]
**Output:** 4
**Explanation:** 
The cell (2, 2) is as far as possible from all the land with distance 4.

**Note:**

1.  `1 <= grid.length == grid[0].length <= 100`
2.  `grid[i][j]` is  `0`  or  `1`

# Solution: BFS
```
class Solution {
    public int maxDistance(int[][] grid) {
        int res = 0;
        int len = grid.length;
        Queue<int[]> queue = new LinkedList<int[]>();
        int[][] neighbors = new int[][]{{-1,0},{1,0},{0,-1},{0,1}};
        
        boolean noWater = true;
        for(int i = 0; i < len; i++){
            for(int j = 0; j < len; j++){
                if(grid[i][j] == 1) {
                    queue.offer(new int[]{i,j});
                }else noWater = false;
            }
        }
        
        if(queue.isEmpty() || noWater) return -1;
            
        while(!queue.isEmpty()){
            int sz = queue.size();
            for(int i = 0; i < sz; i++){
                int[] cur = queue.poll();
                for(int[] neigh:neighbors){
                    int x= cur[0] + neigh[0];
                    int y= cur[1] + neigh[1];
                    if(x >= 0 && x < len && y >= 0 && y < len && grid[x][y] == 0){
                        grid[x][y] = 1;
                        queue.offer(new int[]{x,y});
                    }
                }
            }
            res++;
        }
        
        return res-1;
    }
}
```