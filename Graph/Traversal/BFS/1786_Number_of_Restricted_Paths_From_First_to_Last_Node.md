# 1786. Number of Restricted Paths From First to Last Node
There is an undirected weighted connected graph. You are given a positive integer  `n`  which denotes that the graph has  `n`  nodes labeled from  `1`  to  `n`, and an array  `edges`  where each  `edges[i] = [ui, vi, weighti]`  denotes that there is an edge between nodes  `ui`  and  `vi`  with weight equal to  `weighti`.

A path from node  `start`  to node  `end`  is a sequence of nodes  `[z0, z1,  z2, ..., zk]`  such that  `z0 = start`  and  `zk  = end`  and there is an edge between  `zi`  and  `zi+1`  where  `0 <= i <= k-1`.

The distance of a path is the sum of the weights on the edges of the path. Let  `distanceToLastNode(x)`  denote the shortest distance of a path between node  `n`  and node  `x`. A  **restricted path**  is a path that also satisfies that  `distanceToLastNode(zi) > distanceToLastNode(zi+1)`  where  `0 <= i <= k-1`.

Return  _the number of restricted paths from node_  `1`  _to node_  `n`. Since that number may be too large, return it  **modulo**  `109  + 7`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/17/restricted_paths_ex1.png)

**Input:** n = 5, edges = [[1,2,3],[1,3,3],[2,3,1],[1,4,2],[5,2,2],[3,5,1],[5,4,10]]
**Output:** 3
**Explanation:** Each circle contains the node number in black and its `distanceToLastNode value in blue.` The three restricted paths are:
1) 1 --> 2 --> 5
2) 1 --> 2 --> 3 --> 5
3) 1 --> 3 --> 5

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/17/restricted_paths_ex22.png)

**Input:** n = 7, edges = [[1,3,1],[4,1,2],[7,3,4],[2,5,3],[5,6,1],[6,7,2],[7,5,3],[2,6,4]]
**Output:** 1
**Explanation:** Each circle contains the node number in black and its `distanceToLastNode value in blue.` The only restricted path is 1 --> 3 --> 7.

**Constraints:**

-   `1 <= n <= 2 * 104`
-   `n - 1 <= edges.length <= 4 * 104`
-   `edges[i].length == 3`
-   `1 <= ui, vi  <= n`
-   `ui != vi`
-   `1 <= weighti  <= 105`
-   There is at most one edge between any two nodes.
-   There is at least one path between any two nodes.

# Solution: Dijistra's Algo
```
class Solution {
    class Point{
        int x;
        int dis;
        
        public Point(int _x, int _dis){
            this.x = _x;
            this.dis = _dis;
        }
    }
    
    public void parse_edges(List<int[]>[] edges, int[][] all_edges, int n){
        for(int i = 1; i <= n; i++) edges[i] = new ArrayList<int[]>();
        for(int[] edge: all_edges){
            edges[edge[0]].add(new int[]{edge[1], edge[2]});
            edges[edge[1]].add(new int[]{edge[0], edge[2]});
        }
    }
    
    int MOD = 1000000000+7;
    public int countRestrictedPaths(int n, int[][] all_edges) {
        if(n == 1) return 1;
        List[] edges = new List[n+1];
        parse_edges(edges, all_edges, n);
        
        int[] dis = new int[n+1];
        int[] dp = new int[n+1];
        dijistra_algo(edges, dis, n, dp);
        
        return dp[1];
    }
    
    public void dijistra_algo(List[] edges, int[] dis, int n, int[] dp){
        PriorityQueue<Point> pq = new PriorityQueue<Point>((a,b)->(a.dis-b.dis));
        
        Arrays.fill(dis, Integer.MAX_VALUE);
        dis[n] = 0; dp[n] = 1; 
        pq.offer(new Point(n,0));
        
        while(!pq.isEmpty()){
            Point cur = pq.poll();
            if(cur.dis != dis[cur.x]) continue;
            
            for(int i = 0; i < edges[cur.x].size(); i++){
                int[] edge = (int[]) edges[cur.x].get(i);
                int _x = edge[0];
                
                int _dis = edge[1] + cur.dis;
                if(dp[_x] != 0 && dis[cur.x] > dis[_x]){
                    dp[cur.x] = (dp[cur.x] + dp[_x])%MOD;
                    continue;
                }
                if(_dis >= dis[_x]) continue;
                
                dis[_x] = _dis;
                pq.offer(new Point(_x,_dis));
            }
            
            if(cur.x == 1) return;
        }
    }
    
}
```
