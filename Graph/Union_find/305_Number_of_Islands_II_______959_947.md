# 305. Number of Islands II
A 2d grid map of  `m`  rows and  `n`  columns is initially filled with water. We may perform an  _addLand_  operation which turns the water at position (row, col) into a land. Given a list of positions to operate,  **count the number of islands after each  _addLand_  operation**. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example:**

**Input:** m = 3, n = 3, positions = [[0,0], [0,1], [1,2], [2,1]]
**Output:** [1,1,2,3]

**Explanation:**

Initially, the 2d grid  `grid`  is filled with water. (Assume 0 represents water and 1 represents land).

0 0 0
0 0 0
0 0 0

Operation #1: addLand(0, 0) turns the water at grid[0][0] into a land.

1 0 0
0 0 0   Number of islands = 1
0 0 0

Operation #2: addLand(0, 1) turns the water at grid[0][1] into a land.

1 1 0
0 0 0   Number of islands = 1
0 0 0

Operation #3: addLand(1, 2) turns the water at grid[1][2] into a land.

1 1 0
0 0 1   Number of islands = 2
0 0 0

Operation #4: addLand(2, 1) turns the water at grid[2][1] into a land.

1 1 0
0 0 1   Number of islands = 3
0 1 0

**Follow up:**

Can you do it in time complexity O(k log mn), where k is the length of the  `positions`?


# Solution: Union Find (Beat 97%)
```
class Solution {
    int islands = 0;
    int[] sz;
    int[][] neighbors = new int[][]{{-1,0},{1,0},{0,-1},{0,1}};
    public List<Integer> numIslands2(int m, int n, int[][] positions) {
        List<Integer> res = new ArrayList<Integer>();
        int[] map = new int[m*n];
        sz = new int[m*n];
        for(int i = 0; i < m*n; i++) map[i] = -1;
        
        for(int[] pos: positions){
            int cur_index = calculate_index(pos[0],pos[1],n);
            if(map[cur_index] >= 0) {
                res.add(islands);
                continue;
            }
            map[cur_index] = cur_index;
            
            islands++;
            for(int[] neigh: neighbors){
                int _x = pos[0] + neigh[0];
                int _y = pos[1] + neigh[1];
                if(_x < 0 || _y < 0 || _x >= m || _y >= n) continue;
                int neigh_index = calculate_index(_x,_y,n);
                if(map[neigh_index] == -1 ) continue;
                union(map, cur_index, neigh_index);
            }
            
            res.add(islands);
        }
        
        
        return res;
    }
    
    public void union(int[] map, int p, int q){
        int yp = find(map,p);
        int yq = find(map,q);
        if(yp != yq){
            if(sz[yp] > sz[yq]){
                map[yq] = yp;
            }else{
                map[yp] = yq;
                sz[yq] = Math.max(sz[yp] + 1,sz[yq]);
            }
            
            islands--;
        }
    }
    
    
    public int calculate_index(int x, int y, int n){
        return x*n + y;
    }
    
    public int find(int[] map, int p){
        while(map[p] != p) {
            map[p] = map[map[p]];
            p = map[p];
        }
        return p;
    }
}
```
