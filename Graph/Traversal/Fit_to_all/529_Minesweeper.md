# 529. Minesweeper
Let's play the minesweeper game ([Wikipedia](https://en.wikipedia.org/wiki/Minesweeper_(video_game)),  [online game](http://minesweeperonline.com/))!

You are given a 2D char matrix representing the game board.  **'M'**  represents an  **unrevealed**  mine,  **'E'**  represents an  **unrevealed**  empty square,  **'B'**  represents a  **revealed**  blank square that has no adjacent (above, below, left, right, and all 4 diagonals) mines,  **digit**  ('1' to '8') represents how many mines are adjacent to this  **revealed**  square, and finally  **'X'**  represents a  **revealed**  mine.

Now given the next click position (row and column indices) among all the  **unrevealed**  squares ('M' or 'E'), return the board after revealing this position according to the following rules:

1.  If a mine ('M') is revealed, then the game is over - change it to  **'X'**.
2.  If an empty square ('E') with  **no adjacent mines**  is revealed, then change it to revealed blank ('B') and all of its adjacent  **unrevealed**  squares should be revealed recursively.
3.  If an empty square ('E') with  **at least one adjacent mine**  is revealed, then change it to a digit ('1' to '8') representing the number of adjacent mines.
4.  Return the board when no more squares will be revealed.

**Example 1:**

**Input:** 

[['E', 'E', 'E', 'E', 'E'],
 ['E', 'E', 'M', 'E', 'E'],
 ['E', 'E', 'E', 'E', 'E'],
 ['E', 'E', 'E', 'E', 'E']]

Click : [3,0]

**Output:** 

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'M', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]

**Explanation:**
![](https://assets.leetcode.com/uploads/2018/10/12/minesweeper_example_1.png)

**Example 2:**

**Input:** 

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'M', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]

Click : [1,2]

**Output:** 

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'X', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]

**Explanation:**
![](https://assets.leetcode.com/uploads/2018/10/12/minesweeper_example_2.png)

**Note:**

1.  The range of the input matrix's height and width is [1,50].
2.  The click position will only be an unrevealed square ('M' or 'E'), which also means the input board contains at least one clickable square.
3.  The input board won't be a stage when game is over (some mines have been revealed).
4.  For simplicity, not mentioned rules should be ignored in this problem. For example, you  **don't**  need to reveal all the unrevealed mines when the game is over, consider any cases that you will win the game or flag any squares.

# Solution 1: BFS
```
class Solution {
    public char[][] updateBoard(char[][] board, int[] click) {
        if(board.length == 0 || board[0].length == 0) return board;
        if(board[click[0]][click[1]] == 'M'){
            board[click[0]][click[1]] = 'X';
            return board;
        }        
        
        int row = board.length;
        int col = board[0].length;
        
        Queue<int[]> queue = new LinkedList<int[]>();
        boolean[][] visited = new boolean[row][col];
        queue.offer(click);
        
        while(!queue.isEmpty()){
            click = queue.poll();
            int x = click[0], y = click[1];
            int count = 0;
            for(int j = y-1<0?0:y-1; j<y+2 && j < col; j++){
                for(int i = x-1<0?0:x-1; i < x+2 && i < row; i++){
                    if(board[i][j] == 'M') count++;
                }
            }
            
            if(count == 0){
                board[x][y] = 'B';
                for(int j = y-1<0?0:y-1; j<y+2 && j < col; j++){
                    for(int i = x-1<0?0:x-1; i < x+2 && i < row; i++){
                        if(board[i][j] == 'E' && !visited[i][j]){
                            queue.offer(new int[]{i,j});
                            visited[i][j] = true;
                        } 
                    }
                }
            }else board[x][y] = (char)(count+48);    
        }
        
        return board;
    }
}
```

# Solution 2: DFS Iteration 
Simply change above queue into stack

# Solution 3: DFS Recursion
```
class Solution {
    public char[][] updateBoard(char[][] board, int[] click) {
        if(board.length == 0 || board[0].length == 0) return board;
        if(board[click[0]][click[1]] == 'M'){
            board[click[0]][click[1]] = 'X';
            return board;
        }        
        
        dfs(board,click[0],click[1]);
        
        return board;
    }
    
    public void dfs(char[][] board, int x, int y){
        if(x >= board.length || y >= board[0].length || board[x][y] != 'E') return;
        int count = 0;
        for(int j = y-1<0?0:y-1; j<y+2 && j < board[0].length; j++){
            for(int i = x-1<0?0:x-1; i < x+2 && i < board.length; i++){
                if(board[i][j] == 'M') count++;
            }
        }
        
        if(count == 0){
            board[x][y] = 'B';
            for(int j = y-1<0?0:y-1; j<y+2 && j < board[0].length; j++){
                for(int i = x-1<0?0:x-1; i < x+2 && i < board.length; i++){
                    dfs(board, i, j);
                }
            }
        } else board[x][y] = (char)(count + '0');
        
    }
}
```