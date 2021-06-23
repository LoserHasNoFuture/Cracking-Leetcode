# 37. Sudoku Solver
Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy  **all of the following rules**:

1.  Each of the digits `1-9`  must occur exactly once in each row.
2.  Each of the digits `1-9` must occur exactly once in each column.
3.  Each of the digits `1-9`  must occur exactly once in each of the 9  `3x3`  sub-boxes of the grid.

The  `'.'`  character indicates empty cells.

**Example 1:**

![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

**Input:** board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
**Output:** [["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
**Explanation:** The input board is shown above and the only valid solution is shown below:

![](https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)

**Constraints:**

-   `board.length == 9`
-   `board[i].length == 9`
-   `board[i][j]`  is a digit or  `'.'`.
-   It is  **guaranteed**  that the input board has only one solution.

# Solution 1: Backtracking (Beat 82%)
```
class Solution {
    
    int total = 80; // total number of index;
    public void solveSudoku(char[][] board) {
        dfs(board, 0);
    }
    
    public boolean dfs(char[][] board, int cur){
        if(cur == 81) return true;
        
        int x = cur/9, y = cur%9;
        if(board[x][y] == '.'){
            for(int i = 1; i <= 9; i++){
                if(isValid(board, x, y, i)){
                    board[x][y] = (char)(i + '0');
                    if(dfs(board,cur+1)) return true;
                    board[x][y] = '.';
                }
            }
        }else if(dfs(board, cur + 1)) return true;
        
        return false;
    }
    
    public boolean isValid(char[][] board, int x, int y, int val){
        char c = (char)(val + '0');
        for(int i = 0; i < 9; i++){
            if(board[x][i] == c) return false;
        }
        
        for(int i = 0; i < 9; i++){
            if(board[i][y] == c) return false;
        }
        
        x = x/3; y = y/3;
        for(int i = x*3; i < (x+1) * 3; i++){
            for(int j = y*3; j < (y+1) * 3; j++){
                if(board[i][j] == c) return false;
            }
        }
        
        return true;
    }
    
}
```