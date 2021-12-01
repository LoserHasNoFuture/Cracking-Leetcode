# 1102. Path With Maximum Minimum Value
Given an  `m x n`  integer matrix  `grid`, return  _the maximum  **score**  of a path starting at_ `(0, 0)` _and ending at_ `(m - 1, n - 1)`  moving in the 4 cardinal directions.

The  **score**  of a path is the minimum value in that path.

-   For example, the score of the path  `8 → 4 → 5 → 9`  is  `4`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/08/05/maxgrid1.jpg)

**Input:** grid = [[5,4,5],[1,2,6],[7,4,6]]
**Output:** 4
**Explanation:** The path with the maximum score is highlighted in yellow. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/08/05/maxgrid2.jpg)

**Input:** grid = [[2,2,1,2,2,2],[1,2,2,2,1,2]]
**Output:** 2

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/08/05/maxgrid3.jpg)

**Input:** grid = [[3,4,6,3,4],[0,2,1,1,7],[8,8,3,2,7],[3,2,4,9,8],[4,1,2,0,0],[4,6,5,4,3]]
**Output:** 3

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `1 <= m, n <= 100`
-   `0 <= grid[i][j] <= 109`

# Solution: Dijikstra
```
class Solution {
    class Node implements Comparable<Node>{
        int x;
        int y;
        int score;
        
        public Node(int _x, int _y, int _score){
            this.x = _x;
            this.y = _y;
            this.score = _score;
        }
        
        @Override
        public int compareTo(Node that){
            return that.score - this.score;
        }
    }
    
    public int maximumMinimumPath(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        boolean[][] visited = new boolean[m][n];
        int[][] neighbors = new int[][]{{-1,0},{1,0},{0,1},{0,-1}};
        
        PriorityQueue<Node> pq = new PriorityQueue<Node>();
        pq.offer(new Node(0,0,grid[0][0]));
        
        while(!pq.isEmpty()){
            Node cur = pq.poll();
            visited[cur.x][cur.y] = true;
            
            if(cur.x == m - 1 && cur.y == n-1) return cur.score;
            
            for(int[] neigh: neighbors){
                int _x = cur.x+neigh[0];
                int _y = cur.y+neigh[1];
                if(_x < 0 || _y < 0 || _x >= m || _y >= n || visited[_x][_y]) continue;
                pq.offer(new Node(_x,_y, Math.min(cur.score, grid[_x][_y])));
            }
        }
        
        return Integer.MAX_VALUE;
    }
}
```


# Solution 2 & 3:
1. 不太明显的dijikstra，因为这个题是找最大值。 本质上就是证明：目前greedy选取的最大值，不会在后来找到一个在这个位置更大的。 

2. 用binary search做。首先把所有大于Math.min(begin,end)的点都去掉。 对剩下的点进行排序。然后利用binary search去猜答案，对于每个答案， 用dfs check一下是否存在path，使得该答案是path中的minmum value 

3. 用union-find。首先对所有的点进行反向排序。 然后遍历这些点，每遍历一个，就标记visited。 然后如果它周围也有visited的点，那么就union到一起。 当start 和 end的root相等的时候，当前点的值就是最终的答案。