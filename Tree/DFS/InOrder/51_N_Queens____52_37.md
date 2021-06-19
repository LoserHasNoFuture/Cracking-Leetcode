# 51. N-Queens
The  **n-queens**  puzzle is the problem of placing  `n`  queens on an  `n x n`  chessboard such that no two queens attack each other.

Given an integer  `n`, return  _all distinct solutions to the  **n-queens puzzle**_. You may return the answer in  **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where  `'Q'`  and  `'.'`  both indicate a queen and an empty space, respectively.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

**Input:** n = 4
**Output:** [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
**Explanation:** There exist two distinct solutions to the 4-queens puzzle as shown above

**Example 2:**

**Input:** n = 1
**Output:** [["Q"]]

**Constraints:**

-   `1 <= n <= 9`

# Solution 1: Classical BackTracking Solution (Beat 65%)
```
class Solution {
    List<List<String>> res = new ArrayList<List<String>>();
    int[][] sides = new int[][]{{1,1},{1,-1},{-1,1},{-1,-1}};
    public List<List<String>> solveNQueens(int n) {
        char[][] map = new char[n][n];
        for(int i = 0; i < n; i++){
            for(int j = 0; j < n; j++) map[i][j] = '.';
        }
        
        dfs(map, 0);
        return res;
    }
    
    public void dfs(char[][] map, int index){
        if(index == map.length){
            List<String> arr = new ArrayList<String>();
            for(int i = 0; i < map.length; i++){
                arr.add(new String(map[i]));
            }
            res.add(arr);
            return;
        }
        
        for(int i = 0; i < map.length; i++){
            if(!isValid(map,index,i)) continue;
            map[index][i] = 'Q';
            dfs(map,index+1);
            map[index][i] = '.';
        }
    }
    
    public boolean isValid(char[][] map, int row, int col){
        for(int i = 0; i < map.length; i++){
            //  if(map[row][i] == 'Q') return false;  // we have checked the row
            if(map[i][col] == 'Q') return false;
        }
        
        // similarly, we can modify the following code and only check the above diagons
        for(int[] side: sides){
            int _x = row + side[0];
            int _y = col + side[1];
            while(_x >= 0 && _y >= 0 && _x < map.length && _y < map.length){
                if(map[_x][_y] == 'Q') return false;
                _x += side[0];
                _y += side[1];
            }
        }
        
        return true;
    }
}
```


# Solution 2: BackTracking with Memorization (Beat 100%)
```
class Solution {
    List<List<String>> res = new ArrayList<List<String>>();
    boolean[] dia1, dia2;
    boolean[] cols;
    public List<List<String>> solveNQueens(int n) {
        char[][] map = new char[n][n];
        
        cols = new boolean[n];
        dia1 = new boolean[2*n];
        dia2 = new boolean[2*n];
        
        for(int i = 0; i < n; i++){
            for(int j = 0; j < n; j++) map[i][j] = '.';
        }
        
        dfs(map, 0);
        return res;
    }
    
    public void dfs(char[][] map, int index){
        if(index == map.length){
            List<String> arr = new ArrayList<String>();
            for(int i = 0; i < map.length; i++){
                arr.add(new String(map[i]));
            }
            res.add(arr);
            return;
        }
        
        for(int i = 0; i < map.length; i++){
            int id1 = index + i;
            int id2 = index - i + map.length;
            if(!isValid(i, id1, id2)) continue;
            
            cols[i] = true; dia1[id1] = true; dia2[id2] = true;
            map[index][i] = 'Q';
            
            dfs(map,index+1);
            
            cols[i] = false; dia1[id1] = false; dia2[id2] = false;
            map[index][i] = '.';
        }
    }
    
    public boolean isValid(int col, int id1, int id2){
        
        if(cols[col] || dia1[id1] || dia2[id2]) return false;

        return true;
    }
}
```