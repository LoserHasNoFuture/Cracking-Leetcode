# 959. Regions Cut By Slashes 947 200
In a N x N `grid`  composed of 1 x 1 squares, each 1 x 1 square consists of a  `/`,  `\`, or blank space. These characters divide the square into contiguous regions.

(Note that backslash characters are escaped, so a  `\` is represented as  `"\\"`.)

Return the number of regions.

**Example 1:**

**Input:** [
  " /",
  "/ "
]
**Output:** 2
**Explanation:** The 2x2 grid is as follows:
![](https://assets.leetcode.com/uploads/2018/12/15/1.png)

**Example 2:**

**Input:** [
  " /",
  "  "
]
**Output:** 1
**Explanation:** The 2x2 grid is as follows:
![](https://assets.leetcode.com/uploads/2018/12/15/2.png)

**Example 3:**

**Input:** [
  "\\/",
  "/\\"
]
**Output:** 4
**Explanation:** (Recall that because \ characters are escaped, "\\/" refers to \/, and "/\\" refers to /\.)
The 2x2 grid is as follows:
![](https://assets.leetcode.com/uploads/2018/12/15/3.png)

**Example 4:**

**Input:** [
  "/\\",
  "\\/"
]
**Output:** 5
**Explanation:** (Recall that because \ characters are escaped, "/\\" refers to /\, and "\\/" refers to \/.)
The 2x2 grid is as follows:
![](https://assets.leetcode.com/uploads/2018/12/15/4.png)

**Example 5:**

**Input:** [
  "//",
  "/ "
]
**Output:** 3
**Explanation:** The 2x2 grid is as follows:
![](https://assets.leetcode.com/uploads/2018/12/15/5.png)

**Note:**

1.  `1 <= grid.length == grid[0].length <= 30`
2.  `grid[i][j]`  is either  `'/'`,  `'\'`, or  `' '`.

# Solution 1: Convert it to a graph + DFS (Beat 14%)
Refer from: [https://leetcode.com/problems/regions-cut-by-slashes/discuss/205674/C%2B%2B-with-picture-DFS-on-upscaled-grid](https://leetcode.com/problems/regions-cut-by-slashes/discuss/205674/C%2B%2B-with-picture-DFS-on-upscaled-grid)

```
class Solution {
    
    int[][] neighbors = new int[][]{{1,0},{-1,0},{0,1},{0,-1}};
    public int regionsBySlashes(String[] grid) {
        int n  = grid.length;
        int[][] map = new int[n*3][n*3];
        
        for(int i = 0; i < n; i++){
            char[] word = grid[i].toCharArray();
            for(int j = 0; j < n; j++){
                if(word[j] == '/'){
                    map[i*3][j*3+2] = 1;
                    map[i*3+1][j*3+1] = 1;
                    map[i*3+2][j*3] = 1;
                }
                if(word[j] == '\\'){
                   map[i*3][j*3] = 1;
                    map[i*3+1][j*3+1] = 1;
                    map[i*3+2][j*3+2] = 1;
                }
            }
        }
        
        int cnt = 0;
        
        for(int i = 0; i < n*3; i++){
            for(int j = 0; j < n*3; j++){
                if(map[i][j] == 0){
                    map[i][j] = 1;
                    dfs(map,i,j);
                    cnt++;
                }
            }
        }
        
        return cnt;
    }
    
    public void dfs(int[][] map, int x, int y){
        int m = map.length;
        for(int[] neigh: neighbors){
            int _x = x + neigh[0];
            int _y = y + neigh[1];
            
            if(_x < 0 || _y < 0 || _x >= m || _y >= m || map[_x][_y] == 1) continue;
            map[_x][_y] = 1;
            dfs(map,_x,_y);
        }
    }
}
```

# Solution 2: Union Find (Beat 89%)
Refer From: [https://leetcode.com/problems/regions-cut-by-slashes/discuss/205680/JavaC%2B%2BPython-Split-4-parts-and-Union-Find](https://leetcode.com/problems/regions-cut-by-slashes/discuss/205680/JavaC%2B%2BPython-Split-4-parts-and-Union-Find)

```
class Solution {
    
    int cnt,n;
    int[] map;
    public int regionsBySlashes(String[] grid) {
        n  = grid.length;
        cnt = n * n * 4;
        map = new int[n*n*4];
        for(int i = 0; i < n * n * 4; i++) map[i] = i;
        
        for(int i = 0; i < n; i++){
            for(int j = 0; j < n; j++){
                if(i > 0) union( calculate_position(i,j,3) , calculate_position(i-1,j,1));
                if(j > 0) union( calculate_position(i,j,0) , calculate_position(i,j-1,2));
                if(grid[i].charAt(j) != '\\'){
                    union(calculate_position(i,j,0),calculate_position(i,j,3));
                    union(calculate_position(i,j,1),calculate_position(i,j,2));
                }
                if(grid[i].charAt(j) != '/'){
                    union(calculate_position(i,j,0),calculate_position(i,j,1));
                    union(calculate_position(i,j,3),calculate_position(i,j,2));
                }
            }
        }
        
        return cnt;
    }
    
    public int calculate_position(int i, int j, int pos){
        return (i * n + j) * 4 + pos; 
    }
    
    public void union(int p, int q){
        int yp = find(p);
        int yq = find(q);
        if(yp != yq){
            map[yq] = yp;
            cnt--;
        }
    }
    
    public int find(int p){
        while(map[p] != p) p = map[p];
        return p;
    }
}
```