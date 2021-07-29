# 323. Number of Connected Components in an Undirected Graph
You have a graph of  `n`  nodes. You are given an integer  `n`  and an array  `edges`  where  `edges[i] = [ai, bi]`  indicates that there is an edge between  `ai`  and  `bi`  in the graph.

Return  _the number of connected components in the graph_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/conn1-graph.jpg)

**Input:** n = 5, edges = [[0,1],[1,2],[3,4]]
**Output:** 2

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/conn2-graph.jpg)

**Input:** n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]
**Output:** 1

**Constraints:**

-   `1 <= n <= 2000`
-   `1 <= edges.length <= 5000`
-   `edges[i].length == 2`
-   `0 <= ai  <= bi  < n`
-   `ai  != bi`
-   There are no repeated edges.

# Solution: Union Find (Beat 100%)
```
class Solution {
    public int countComponents(int n, int[][] edges) {
        int[] id = new int[n], sz = new int[n];
        
        for(int i = 0; i < n; i++) id[i] = i;
        int res = n;
        
        for(int[] edge: edges){
            int root1 = find_root(id,edge[0]);
            int root2 = find_root(id,edge[1]);
            if(root1 == root2) continue;
            res--;
            
            if(sz[root1] > sz[root2]) id[root2] = root1;
            else{
                id[root1] = root2;
                sz[root2] = Math.max(sz[root2],sz[root1] + 1);
            }
        }
        
        return res;
    }
    
    public int find_root(int[] id, int p){
        while(p != id[p]){
            id[p] = id[id[p]];
            p = id[p];
        }
        return p;
    }
}
```