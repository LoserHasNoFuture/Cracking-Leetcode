# 1579. Remove Max Number of Edges to Keep Graph Fully Traversable
Alice and Bob have an undirected graph of `n` nodes and 3 types of edges:

-   Type 1: Can be traversed by Alice only.
-   Type 2: Can be traversed by Bob only.
-   Type 3: Can by traversed by both Alice and Bob.

Given an array `edges` where `edges[i] = [typei, ui, vi]` represents a bidirectional edge of type `typei` between nodes `ui` and `vi`, find the maximum number of edges you can remove so that after removing the edges, the graph can still be fully traversed by both Alice and Bob. The graph is fully traversed by Alice and Bob if starting from any node, they can reach all other nodes.

Return  _the maximum number of edges you can remove, or return_  `-1`  _if it's impossible for the graph to be fully traversed by Alice and Bob._

**Example 1:**

**![](https://assets.leetcode.com/uploads/2020/08/19/ex1.png)**

**Input:** n = 4, edges = [[3,1,2],[3,2,3],[1,1,3],[1,2,4],[1,1,2],[2,3,4]]
**Output:** 2
**Explanation:** If we remove the 2 edges [1,1,2] and [1,1,3]. The graph will still be fully traversable by Alice and Bob. Removing any additional edge will not make it so. So the maximum number of edges we can remove is 2.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2020/08/19/ex2.png)**

**Input:** n = 4, edges = [[3,1,2],[3,2,3],[1,1,4],[2,1,4]]
**Output:** 0
**Explanation:** Notice that removing any edge will not make the graph fully traversable by Alice and Bob.

**Example 3:**

**![](https://assets.leetcode.com/uploads/2020/08/19/ex3.png)**

**Input:** n = 4, edges = [[3,2,3],[1,1,2],[2,3,4]]
**Output:** -1
**Explanation:** In the current graph, Alice cannot reach node 4 from the other nodes. Likewise, Bob cannot reach 1. Therefore it's impossible to make the graph fully traversable.

**Constraints:**

-   `1 <= n <= 10^5`
-   `1 <= edges.length <= min(10^5, 3 * n * (n-1) / 2)`
-   `edges[i].length == 3`
-   `1 <= edges[i][0] <= 3`
-   `1 <= edges[i][1] < edges[i][2] <= n`
-   All tuples `(typei, ui, vi)` are distinct.

# Solution: Union Find
```
class Solution {
    int res = 0;
    public int maxNumEdgesToRemove(int n, int[][] edges) {
        int[] id = new int[n+1], sz = new int[n+1];
        for(int i = 1; i <= n; i++) id[i] = i;
        
        int used = connect_edge(n, edges, id, sz, 3, 0);
        
        int used2 = used;
        int[] id2 = new int[n+1], sz2 = new int[n+1];
        for(int i = 1; i <= n; i++){
            id2[i] = id[i];
            sz2[i] = sz[i];
        }
        
        used = connect_edge(n, edges, id, sz, 1, used);
        // System.out.println(used);
        if(used != n-1) return -1;
        
        used2 = connect_edge(n, edges, id2, sz2, 2, used2);
        // System.out.println(used2);
        if(used2 != n-1) return -1;
        
        return res;
    }
    
    public int connect_edge(int n, int[][] edges, int[] id, int[] sz, int type, int used){
        for(int[] edge: edges){
            if(edge[0] == type){
                int r1 = find_root(id, edge[1]);
                int r2 = find_root(id, edge[2]);
                if(r1 == r2) {
                    res++; continue;
                }
                used++;
                if(sz[r1] > sz[r2]) id[r2] = r1;
                else{
                    id[r1] = r2;
                    sz[r2] = Math.max(sz[r2],sz[r1]+1);
                }
            }
        }
        return used;
    }
    
    
    public int find_root(int[] id, int p){
        while(id[p] != p){
            id[p] = id[id[p]];
            p = id[p];
        }
        return p;
    }
}
```