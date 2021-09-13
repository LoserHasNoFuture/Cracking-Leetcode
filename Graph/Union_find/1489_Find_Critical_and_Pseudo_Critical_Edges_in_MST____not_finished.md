# 1489. Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree  (not finished)
Given a weighted undirected connected graph with  `n` vertices numbered from  `0`  to  `n - 1`, and an array  `edges` where  `edges[i] = [ai, bi, weighti]`  represents a bidirectional and weighted edge between nodes `ai` and  `bi`. A minimum spanning tree (MST) is a subset of the graph's edges that connects all vertices without cycles and with the minimum possible total edge weight.

Find  _all the critical and pseudo-critical edges in the given graph's minimum spanning tree (MST)_. An MST edge whose deletion from the graph would cause the MST weight to increase is called a _critical edge_. On the other hand, a pseudo-critical edge is that which can appear in some MSTs but not all.

Note that you can return the indices of the edges in any order.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/06/04/ex1.png)

**Input:** n = 5, edges = [[0,1,1],[1,2,1],[2,3,2],[0,3,2],[0,4,3],[3,4,3],[1,4,6]]
**Output:** [[0,1],[2,3,4,5]]
**Explanation:** The figure above describes the graph.
The following figure shows all the possible MSTs:
![](https://assets.leetcode.com/uploads/2020/06/04/msts.png)
Notice that the two edges 0 and 1 appear in all MSTs, therefore they are critical edges, so we return them in the first list of the output.
The edges 2, 3, 4, and 5 are only part of some MSTs, therefore they are considered pseudo-critical edges. We add them to the second list of the output.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/06/04/ex2.png)

**Input:** n = 4, edges = [[0,1,1],[1,2,1],[2,3,1],[0,3,1]]
**Output:** [[],[0,1,2,3]]
**Explanation:** We can observe that since all 4 edges have equal weight, choosing any 3 edges from the given 4 will yield an MST. Therefore all 4 edges are pseudo-critical.

**Constraints:**

-   `2 <= n <= 100`
-   `1 <= edges.length <= min(200, n * (n - 1) / 2)`
-   `edges[i].length == 3`
-   `0 <= ai  < bi  < n`
-   `1 <= weighti <= 1000`
-   All pairs  `(ai, bi)`  are  **distinct**.


# Solution 1: Union Find Edge by Edge O(ElogE)
```
class Solution {
    List<Integer> critical = new ArrayList<Integer>();
    List<Integer> pseudo = new ArrayList<Integer>();
    int m;
    public List<List<Integer>> findCriticalAndPseudoCriticalEdges(int n, int[][] ori_edges) {
        m = ori_edges.length;
        int[][] edges = new int[m][4];
        
        for(int i = 0; i < m; i++){
            edges[i][0] = ori_edges[i][0];
            edges[i][1] = ori_edges[i][1];
            edges[i][2] = ori_edges[i][2];
            edges[i][3] = i;
        }
        Arrays.sort(edges, (a,b)->(a[2]-b[2]));
        
        boolean[] used = new boolean[edges.length];
        Union_Find uf = new Union_Find(n);
        int minCost = calculate_MST(edges, used, uf, 0);
        
        for(int i = 0; i < m; i++){        
            used = new boolean[m];
            uf = new Union_Find(n);
            
            used[i] = true;
            int woCost = calculate_MST(edges, used, uf, 0);
            if(woCost > minCost) {
                critical.add(edges[i][3]);
                continue;
            }
            
            uf = new Union_Find(n);
            uf.union(edges[i][0],edges[i][1]);
            int wCost = calculate_MST(edges, used, uf, edges[i][2]);
            
            if(wCost == minCost) pseudo.add(edges[i][3]);
        }
        
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        res.add(critical);
        res.add(pseudo);
        return res;
    }
    
    
    public int calculate_MST(int[][] edges, boolean[] used, Union_Find uf, int cost){
        int cnt = cost > 0? 1: 0;
        for(int i = 0; i < m; i++){
            if(used[i]) continue;
            if(uf.union(edges[i][0],edges[i][1])) {
                cost += edges[i][2];
                cnt++;
            }
        }
        
        return uf.num_of_groups() == 1? cost: Integer.MAX_VALUE;
    }
    
    class Union_Find{
        int cnt;
        int[] id;
        int[] sz;
        
        public Union_Find(int n){
            this.cnt = n;
            this.id = new int[n];
            this.sz = new int[n];
            for(int i = 0; i < n; i++) id[i] = i;
        }
        
        public int num_of_groups(){
            return cnt;
        }
        
        public int find_root(int p){
            while(p != id[p]){
                id[p] = id[id[p]];
                p = id[p];
            }
            return p;
        }
        
        public boolean union(int x, int y){
            int r1 = find_root(x);
            int r2 = find_root(y);
            if(r1 == r2) return false;
            if(sz[r1] > sz[r2]) id[r2] = r1;
            else{
                id[r1] = r2;
                sz[r2] = Math.max(sz[r2], sz[r1] + 1);
            }
            
            cnt--;
            return true;
        }
    }
    
}
```