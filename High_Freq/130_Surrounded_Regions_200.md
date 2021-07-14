# 130. Surrounded Regions 200
Given a 2D board containing  `'X'`  and  `'O'`  (**the letter O**), capture all regions surrounded by  `'X'`.

A region is captured by flipping all  `'O'`s into  `'X'`s in that surrounded region.

**Example:**

X X X X
X O O X
X X O X
X O X X

After running your function, the board should be:

X X X X
X X X X
X X X X
X O X X

**Explanation:**

Surrounded regions shouldn’t be on the border, which means that any  `'O'` on the border of the board are not flipped to  `'X'`. Any  `'O'` that is not on the border and it is not connected to an  `'O'` on the border will be flipped to  `'X'`. Two cells are connected if they are adjacent cells connected horizontally or vertically.

# Solution 1: BFS
```
class Solution {
    public void solve(char[][] board) {
        if( board.length == 0 ||  board[0].length == 0) return;
        int row = board.length;
        int col = board[0].length;
        
        for(int i = 0; i < col; i++){
            if(board[0][i] == 'O') BFS(board,0,i);
            if(board[row-1][i] == 'O') BFS(board,row-1,i);
        }
            
        for(int i = 0; i < row; i++){
            if(board[i][0] == 'O') BFS(board,i,0);
            if(board[i][col-1] == 'O') BFS(board,i,col-1);
        }
        
        for(int i = 0; i < board.length; i++){
            for(int j = 0; j < board[0].length; j++){
                if(board[i][j] == 'O') board[i][j]  = 'X';
                else if(board[i][j] == '#') board[i][j]  = 'O';
            }
        }
    }
    
    public void BFS(char[][] grid, int i, int j){
        Queue<int[]> q = new LinkedList<int[]>();
        q.offer(new int[]{i,j});
        grid[i][j] = '#';
        
        int[][] neighbors = new int[][]{
            {-1,0},{1,0},{0,-1},{0,1}
        };
        
        while(!q.isEmpty()){
            int[] pos = q.poll();
            
            for(int[] n:neighbors){
                int rIndex = pos[0] - n[0];
                int cIndex = pos[1] - n[1];
                if(rIndex < 0 || rIndex >=  grid.length 
                   || cIndex < 0 || cIndex >= grid[0].length
                  || grid[rIndex][cIndex] != 'O') continue;
                grid[rIndex][cIndex] = '#';
                q.offer(new int[]{rIndex, cIndex});
            }   
        }
    }
}
```

# Solution 2: DFS Recursion
Can easily change to iteration
```
class Solution {
    public void solve(char[][] board) {
        if( board.length == 0 ||  board[0].length == 0) return;
        int row = board.length;
        int col = board[0].length;
        
        for(int i = 0; i < col; i++){
            if(board[0][i] == 'O') DFS(board,0,i);
            if(board[row-1][i] == 'O') DFS(board,row-1,i);
        }
            
        for(int i = 0; i < row; i++){
            if(board[i][0] == 'O') DFS(board,i,0);
            if(board[i][col-1] == 'O') DFS(board,i,col-1);
        }
        
        for(int i = 0; i < board.length; i++){
            for(int j = 0; j < board[0].length; j++){
                if(board[i][j] == 'O') board[i][j]  = 'X';
                else if(board[i][j] == '#') board[i][j]  = 'O';
            }
        }
    }
    
    public void DFS(char[][] grid, int i, int j){
        if(i < 0 || i >= grid.length 
          || j < 0 || j >= grid[0].length 
           || grid[i][j] != 'O') return;
        grid[i][j] = '#';
        DFS(grid,i-1,j);
        DFS(grid,i+1,j);
        DFS(grid,i,j-1);
        DFS(grid,i,j+1);
    }
}
```

# Solution 3: Union Find
Copy from discuss section:
```
public class Solution {
    int rows, cols;
    
    public void solve(char[][] board) {
        if(board == null || board.length == 0) return;
        
        rows = board.length;
        cols = board[0].length;
        
        // last one is dummy, all outer O are connected to this dummy
        UnionFind uf = new UnionFind(rows * cols + 1);
        int dummyNode = rows * cols;
        
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(board[i][j] == 'O') {
                    if(i == 0 || i == rows-1 || j == 0 || j == cols-1) {
                        uf.union(node(i,j), dummyNode);
                    }
                    else {
                        if(i > 0 && board[i-1][j] == 'O')  uf.union(node(i,j), node(i-1,j));
                        if(i < rows-1 && board[i+1][j] == 'O')  uf.union(node(i,j), node(i+1,j));
                        if(j > 0 && board[i][j-1] == 'O')  uf.union(node(i,j), node(i, j-1));
                        if(j < cols-1 && board[i][j+1] == 'O')  uf.union(node(i,j), node(i, j+1));
                    }
                }
            }
        }
        
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(uf.isConnected(node(i,j), dummyNode)) {
                    board[i][j] = 'O';
                }
                else {
                    board[i][j] = 'X';
                }
            }
        }
    }
    
    int node(int i, int j) {
        return i * cols + j;
    }
}

class UnionFind {
    int [] parents;
    public UnionFind(int totalNodes) {
        parents = new int[totalNodes];
        for(int i = 0; i < totalNodes; i++) {
            parents[i] = i;
        }
    }
    
    void union(int node1, int node2) {
        int root1 = find(node1);
        int root2 = find(node2);
        if(root1 != root2) {
            parents[root2] = root1;
        }
    }
    
    int find(int node) {
        while(parents[node] != node) {
            parents[node] = parents[parents[node]];
            node = parents[node];
        }
        return node;
    }
    
    boolean isConnected(int node1, int node2) {
        return find(node1) == find(node2);
    }
}
```