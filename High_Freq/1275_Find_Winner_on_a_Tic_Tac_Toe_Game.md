# 1275. Find Winner on a Tic Tac Toe Game
Tic-tac-toe is played by two players  _A_  and  _B_  on a _3_ x _3_ grid.

Here are the rules of Tic-Tac-Toe:

-   Players take turns placing characters into empty squares (" ").
-   The first player  _A_  always places "X" characters, while the second player  _B_ always places "O" characters.
-   "X" and "O" characters are always placed into empty squares, never on filled ones.
-   The game ends when there are 3 of the same (non-empty) character filling any row, column, or diagonal.
-   The game also ends if all squares are non-empty.
-   No more moves can be played if the game is over.

Given an array  `moves`  where each element is another array of size 2 corresponding to the row and column of the grid where they mark their respective character in the order in which  _A_  and  _B_  play.

Return the winner of the game if it exists (_A_  or  _B_), in case the game ends in a draw return "Draw", if there are still movements to play return "Pending".

You can assume that `moves`  is **valid**  (It follows the rules of Tic-Tac-Toe), the grid is initially empty and  _A_  will play  **first**.

**Example 1:**

**Input:** moves = [[0,0],[2,0],[1,1],[2,1],[2,2]]
**Output:** "A"
**Explanation:** "A" wins, he always plays first.
"X  "    "X  "    "X  "    "X  "    "**X**  "
"   " -> "   " -> " X " -> " X " -> " **X** "
"   "    "O  "    "O  "    "OO "    "OO**X**"

**Example 2:**

**Input:** moves = [[0,0],[1,1],[0,1],[0,2],[1,0],[2,0]]
**Output:** "B"
**Explanation:** "B" wins.
"X  "    "X  "    "XX "    "XXO"    "XXO"    "XX**O**"
"   " -> " O " -> " O " -> " O " -> "XO " -> "X**O** " 
"   "    "   "    "   "    "   "    "   "    "**O**  "

**Example 3:**

**Input:** moves = [[0,0],[1,1],[2,0],[1,0],[1,2],[2,1],[0,1],[0,2],[2,2]]
**Output:** "Draw"
**Explanation:** The game ends in a draw since there are no moves to make.
"XXO"
"OOX"
"XOX"

**Example 4:**

**Input:** moves = [[0,0],[1,1]]
**Output:** "Pending"
**Explanation:** The game has not finished yet.
"X  "
" O "
"   "

**Constraints:**

-   `1 <= moves.length <= 9`
-   `moves[i].length == 2`
-   `0 <= moves[i][j] <= 2`
-   There are no repeated elements on  `moves`.
-   `moves`  follow the rules of tic tac toe.

# Solution 1: Straightforward (Beat 20%)
Refer from: [https://leetcode.com/problems/find-winner-on-a-tic-tac-toe-game/discuss/490101/Java-100-with-comments-and-Interview-tip](https://leetcode.com/problems/find-winner-on-a-tic-tac-toe-game/discuss/490101/Java-100-with-comments-and-Interview-tip)
```
class Solution {
    int n = 3;
    public String tictactoe(int[][] moves) {
        char[][] board = new char[n][n];
        for(int i = 0; i < moves.length; i++){
            int row = moves[i][0];
            int col = moves[i][1];
            if((i & 1) == 0){
                //even, X's move
                board[row][col] = 'X';
                if(didWin(board, row, col, 'X')) return "A";
            }else{
                //odd, O's move
                board[row][col] = 'O';
                if(didWin(board, row, col, 'O')) return "B";
            }
        }
        return moves.length == n * n ? "Draw" : "Pending";
    }
    
    private boolean didWin(char[][] board, int row, int col, char player){
        //check the current row
        boolean didPlayerWin = true;
        for(int i = 0; i < n; i++){
            if(board[row][i] != player){
                didPlayerWin = false;
            }                
        }
        if(didPlayerWin) return true;   //player has won the game
        
        //check the current col
        didPlayerWin = true;
        for(int i = 0; i < n; i++){
            if(board[i][col] != player){
                didPlayerWin = false;
            }                
        }
        if(didPlayerWin) return true;   //player has won the game
        
        //check the diagonal
        if(row == col){
            didPlayerWin = true;
            for(int i = 0; i < n; i++){
                if(board[i][i] != player){
                    didPlayerWin = false;
                }                
            }
            if(didPlayerWin) return true;   //player has won the game    
        }
        
        //check reverse diagonal
        if(col == n - row - 1){
            didPlayerWin = true;
            for(int i = 0; i < n; i++){
                if(board[i][n-i-1] != player){
                    didPlayerWin = false;
                }                
            }
            if(didPlayerWin) return true;   //player has won the game    
        }
        
        //player did not win
        return false;
    }
}
```

# Solution 2: Count the number of 'X' and 'O' (Beat 100%)
Refer from: [https://leetcode.com/problems/find-winner-on-a-tic-tac-toe-game/discuss/441319/JavaPython-3-Check-rows-columns-and-two-diagonals-w-brief-explanation-and-analysis.](https://leetcode.com/problems/find-winner-on-a-tic-tac-toe-game/discuss/441319/JavaPython-3-Check-rows-columns-and-two-diagonals-w-brief-explanation-and-analysis.)
```
class Solution {
    public String tictactoe(int[][] moves) {
        int[] aRow = new int[3], aCol = new int[3], bRow = new int[3], bCol = new int[3];
        int  aD1 = 0, aD2 = 0, bD1 = 0, bD2 = 0;
        for (int i = 0; i < moves.length; ++i) {
            int r = moves[i][0], c = moves[i][1];
            if (i % 2 == 1) {
                if (++bRow[r] == 3 || ++bCol[c] == 3 || r == c && ++bD1 == 3 || r + c == 2 && ++bD2 == 3) return "B";
            }else {
                if (++aRow[r] == 3 || ++aCol[c] == 3 || r == c && ++aD1 == 3 || r + c == 2 && ++aD2 == 3) return "A";
            }
        }
        return moves.length == 9 ? "Draw" : "Pending";        
    }
}
```