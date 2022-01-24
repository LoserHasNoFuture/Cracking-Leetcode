# 212. Word Search II
Given an  `m x n`  `board` of characters and a list of strings  `words`, return  _all words on the board_.

Each word must be constructed from letters of sequentially adjacent cells, where  **adjacent cells**  are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/07/search1.jpg)

**Input:** board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
**Output:** ["eat","oath"]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/07/search2.jpg)

**Input:** board = [["a","b"],["c","d"]], words = ["abcb"]
**Output:** []

**Constraints:**

-   `m == board.length`
-   `n == board[i].length`
-   `1 <= m, n <= 12`
-   `board[i][j]`  is a lowercase English letter.
-   `1 <= words.length <= 3 * 104`
-   `1 <= words[i].length <= 10`
-   `words[i]`  consists of lowercase English letters.
-   All the strings of  `words`  are unique.

# Solution: Trie
```
class Solution {
    
    public List<String> findWords(char[][] board, String[] words) {
        List<String> res = new ArrayList<>();
        TrieNode root = buildTrie(words);
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                dfs (board, i, j, root, res);
            }
        }
        return res;
    }

    public void dfs(char[][] board, int i, int j, TrieNode p, List<String> res) {
        char c = board[i][j];
        if (c == '#' || p.next[c - 'a'] == null) return;
        p = p.next[c - 'a'];
        if (p.word != null) {   // found one
            res.add(p.word);
            p.word = null;     // de-duplicate
        }

        board[i][j] = '#';
        if (i > 0) dfs(board, i - 1, j ,p, res); 
        if (j > 0) dfs(board, i, j - 1, p, res);
        if (i < board.length - 1) dfs(board, i + 1, j, p, res); 
        if (j < board[0].length - 1) dfs(board, i, j + 1, p, res); 
        board[i][j] = c;
    }

    public TrieNode buildTrie(String[] words) {
        TrieNode root = new TrieNode();
        for (String w : words) {
            TrieNode p = root;
            for (char c : w.toCharArray()) {
                int i = c - 'a';
                if (p.next[i] == null) p.next[i] = new TrieNode();
                p = p.next[i];
           }
           p.word = w;
        }
        return root;
    }

    class TrieNode {
        TrieNode[] next = new TrieNode[26];
        String word;
    }
}
```


# Solution: Brute Force Back Tracking (Beat 5%)
```
class Solution {
    int max_len = 0;
    HashSet<String> res = new HashSet<String>();
    HashSet<String> set = new HashSet<String>();
    int[][] neighbors = new int[][]{{-1,0},{1,0},{0,-1},{0,1}};
    
    public List<String> findWords(char[][] board, String[] words) {
        int row = board.length;
        int col = board[0].length;
        
        for(int i = 0; i < words.length; i++) {
            max_len = Math.max(max_len, words[i].length());
            set.add(words[i]);
        }
        
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i < row; i++){
            for(int j = 0; j < col; j++){
                sb.append(board[i][j]);
                board[i][j] = (char)(board[i][j]+32);
                dfs(board, 1, i, j, sb);
                board[i][j] = (char)(board[i][j]-32);
                sb.delete(sb.length()-1,sb.length());

            }
        }
        
        ArrayList<String> ret = new ArrayList<String>();
        
        for(String str:res){
            ret.add(str);
        }
        
        return ret;
    }
    
    
    public void dfs(char[][] board, int index, int x, int y, StringBuilder sb){
        
        
        if(index > max_len) return;
        if(set.contains(sb.toString())) res.add(sb.toString());
        
        for(int[] neigh: neighbors){
            int next_x = x + neigh[0];
            int next_y = y + neigh[1];
            if(next_x < 0|| next_y < 0 || 
               next_x >= board.length || next_y >= board[0].length ||
              board[next_x][next_y] < 'a' || board[next_x][next_y] > 'z') continue;
            
            sb.append(board[next_x][next_y]);
            board[next_x][next_y] = (char)(board[next_x][next_y]+32);
            
            dfs(board, index+1, next_x, next_y, sb);
            
            board[next_x][next_y] = (char)(board[next_x][next_y]-32);
            sb.delete(sb.length()-1,sb.length());
            
        }
        
        
    }
}
```
