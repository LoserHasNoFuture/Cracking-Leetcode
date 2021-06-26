# 289. Game of Life
According to [Wikipedia's article](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life): "The  **Game of Life**, also known simply as  **Life**, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

The board is made up of an  `m x n`  grid of cells, where each cell has an initial state:  **live**  (represented by a  `1`) or  **dead**  (represented by a  `0`). Each cell interacts with its  [eight neighbors](https://en.wikipedia.org/wiki/Moore_neighborhood)  (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

1.  Any live cell with fewer than two live neighbors dies as if caused by under-population.
2.  Any live cell with two or three live neighbors lives on to the next generation.
3.  Any live cell with more than three live neighbors dies, as if by over-population.
4.  Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously. Given the current state of the  `m x n`  grid  `board`, return  _the next state_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/26/grid1.jpg)

**Input:** board = [[0,1,0],[0,0,1],[1,1,1],[0,0,0]]
**Output:** [[0,0,0],[1,0,1],[0,1,1],[0,1,0]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/26/grid2.jpg)

**Input:** board = [[1,1],[1,0]]
**Output:** [[1,1],[1,1]]

**Constraints:**

-   `m == board.length`
-   `n == board[i].length`
-   `1 <= m, n <= 25`
-   `board[i][j]`  is  `0`  or  `1`.

**Follow up:**

-   Could you solve it in-place? Remember that the board needs to be updated simultaneously: You cannot update some cells first and then use their updated values to update other cells.
-   In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches upon the border of the array (i.e., live cells reach the border). How would you address these problems?

# Solution 1: Encode Result (Beat 100%)
```
class Solution {
    int[][] neighbors = new int[][]{{-1,-1},{1,1},{-1,1},{1,-1},{-1,0},{1,0},{0,-1},{0,1}};
    int row = 0, col = 0;
    public void gameOfLife(int[][] board) {
        row = board.length;
        col = board[0].length;
        
//         calculate the new board
        for(int i = 0; i < row; i++){
            for(int j = 0; j < col; j++){
                calculate_new_board(board, i, j);
            }
        }
        
//         update together
        for(int i = 0; i < row; i++){
            for(int j = 0; j < col; j++){
                update_value(board, i, j);
            }
        }
        
    }
    
    public void update_value(int[][] board, int x, int y){
        if(board[x][y]%2 == 0){
            if(board[x][y] == 2) board[x][y] = 1;
            else board[x][y] = 0;
        }else{
            if(board[x][y] == 3) board[x][y] = 0;
            else board[x][y] = 1;
        }
    }
    
    public void calculate_new_board(int[][] board, int x, int y){
        int cnt_live = 0;
        for(int[] neigh: neighbors){
            int _x = x + neigh[0];
            int _y = y + neigh[1];
            if(_x < 0 || _y < 0 || _x >= row || _y >= col || board[_x][_y]%2 == 0) continue;
            else cnt_live++;
        }
        
        if(board[x][y] == 1){
            if(cnt_live < 2 || cnt_live > 3) board[x][y] = 3; // from 1 to 0
            // else board[x][y] = 1;  // remain 1
        }else{
            if(cnt_live == 3) board[x][y] = 2;  // from 0 to 1
            // else board[x][y] = 0;  // remain 0
        }
    }
}
```

如果这道题限制是boolean,那么是没法做到in-place变化的.
至少需要O(Min(m,n))的空间
存下一行的结果,等到它的下一行更新完之后,再update它的

# Follow UP 2: 没读懂题
# Follow UP 3:
From discussion section, how to solve this problem using multi-threading?

My conjecture: Divide and Conquer?
