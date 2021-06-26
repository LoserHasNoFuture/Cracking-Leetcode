# 36. Valid Sudoku
Determine if a `9 x 9`  Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

1.  Each row must contain the digits `1-9`  without repetition.
2.  Each column must contain the digits `1-9` without repetition.
3.  Each of the nine `3 x 3`  sub-boxes of the grid must contain the digits `1-9` without repetition.

**Note:**

-   A Sudoku board (partially filled) could be valid but is not necessarily solvable.
-   Only the filled cells need to be validated according to the mentioned rules.

**Example 1:**

![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

**Input:** board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
**Output:** true

**Example 2:**

**Input:** board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
**Output:** false
**Explanation:** Same as Example 1, except with the **5** in the top left corner being modified to **8**. Since there are two 8's in the top left 3x3 sub-box, it is invalid.

**Constraints:**

-   `board.length == 9`
-   `board[i].length == 9`
-   `board[i][j]`  is a digit or  `'.'`.

# Solution 1:Straight Forward (Beat 90%)
```
class Solution {
    public boolean isValidSudoku(char[][] board) {
        HashSet<Character> set = new HashSet<Character>();
        
        for(int i = 0; i < 9; i++){
            set.clear();
            for(int j = 0; j < 9; j++){
                if(board[i][j] != '.'){
                    if(set.contains(board[i][j])) return false;
                    else set.add(board[i][j]);
                }
            }
        }
        
        for(int j = 0; j < 9; j++){
            set.clear();
            for(int i = 0; i < 9; i++){
                if(board[i][j] != '.'){
                    if(set.contains(board[i][j])) return false;
                    else set.add(board[i][j]);
                }
            }
        }
        
        for(int i = 0; i <= 6; i = i+3){
            for(int j = 0; j <= 6; j = j+3){
                set.clear();
                for(int m = i; m < i + 3; m++){
                    for(int k = j; k < j + 3; k++){
                        if(board[m][k] != '.'){
                            if(set.contains(board[m][k])) return false;
                            else set.add(board[m][k]);
                        }
                    }
                }
                
            }
        }
        
        return true;
    }
}
```

# Solution 2: More Readable
[https://leetcode.com/problems/valid-sudoku/discuss/15472/Short%2BSimple-Java-using-Strings](https://leetcode.com/problems/valid-sudoku/discuss/15472/Short%2BSimple-Java-using-Strings)
It has the same time complexity as Solution 1.
```
class Solution {
    public boolean isValidSudoku(char[][] board) {
        Set seen = new HashSet();
        for (int i=0; i<9; ++i) {
            for (int j=0; j<9; ++j) {
                char number = board[i][j];
                if (number != '.')
                    if (!seen.add(number + " in row " + i) ||
                        !seen.add(number + " in column " + j) ||
                        !seen.add(number + " in block " + i/3 + "-" + j/3))
                        return false;
            }
        }
        return true;
    }
}
```
