# 79. Word Search
Given an `m x n`  `board`  and a  `word`, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where "adjacent" cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

**Input:** board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg)

**Input:** board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
**Output:** true

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/10/15/word3.jpg)

**Input:** board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
**Output:** false

**Constraints:**

-   `m == board.length`
-   `n = board[i].length`
-   `1 <= m, n <= 200`
-   `1 <= word.length <= 103`
-   `board` and  `word`  consists only of lowercase and uppercase English letters.


# Solution 1: BackTracking (Beat 80%)
```
class Solution {
    
    int[][] neighbors = new int[][]{{-1,0},{1,0},{0,-1},{0,1}};
    public boolean exist(char[][] board, String word) {
        int row = board.length;
        int col = board[0].length;
        
        for(int i = 0; i < row; i++){
            for(int j = 0; j < col; j++){
                if(board[i][j] == word.charAt(0)) {
                    board[i][j] = (char)(board[i][j]+32);
                    if(dfs(board, word, 1, i, j)) return true;
                    board[i][j] = (char)(board[i][j]-32);
                }
            }
        }
        
        return false;
    }
    
    public boolean dfs(char[][] board, String word, int index, int x, int y){
        if(index == word.length()) return true;
        
        for(int[] neigh: neighbors){
            int next_x = x + neigh[0];
            int next_y = y + neigh[1];
            if(next_x < 0|| next_y < 0 || 
               next_x >= board.length || next_y >= board[0].length ||
              board[next_x][next_y] != word.charAt(index)) continue;
                
            board[next_x][next_y] = (char)(board[next_x][next_y]+32);
            if(dfs(board, word, index+1, next_x, next_y)) return true;
            board[next_x][next_y] = (char)(board[next_x][next_y]-32);
            
        }
        
        return false;
    }
}
```