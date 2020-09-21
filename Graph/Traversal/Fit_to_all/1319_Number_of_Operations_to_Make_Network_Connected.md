# 1319. Number of Operations to Make Network Connected
There are `n` computers numbered from `0` to `n-1` connected by ethernet cables `connections` forming a network where `connections[i] = [a, b]` represents a connection between computers `a` and `b`. Any computer can reach any other computer directly or indirectly through the network.

Given an initial computer network  `connections`. You can extract certain cables between two directly connected computers, and place them between any pair of disconnected computers to make them directly connected. Return the  _minimum number of times_  you need to do this in order to make all the computers connected. If it's not possible, return -1.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2020/01/02/sample_1_1677.png)**

**Input:** n = 4, connections = [[0,1],[0,2],[1,2]]
**Output:** 1
**Explanation:** Remove cable between computer 1 and 2 and place between computers 1 and 3.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2020/01/02/sample_2_1677.png)**

**Input:** n = 6, connections = [[0,1],[0,2],[0,3],[1,2],[1,3]]
**Output:** 2

**Example 3:**

**Input:** n = 6, connections = [[0,1],[0,2],[0,3],[1,2]]
**Output:** -1
**Explanation:** There are not enough cables.

**Example 4:**

**Input:** n = 5, connections = [[0,1],[0,2],[3,4],[2,3]]
**Output:** 0

**Constraints:**
```
-   `1 <= n <= 10^5`
-   `1 <= connections.length <= min(n*(n-1)/2, 10^5)`
-   `connections[i].length == 2`
-   `0 <= connections[i][0], connections[i][1] < n`
-   `connections[i][0] != connections[i][1]`
-   There are no repeated connections.
-   No two computers are connected by more than one cable.
```
# Solution: Union Find
```
class Solution {
    public int makeConnected(int n, int[][] connections) {
        if(connections.length < n - 1) return - 1;
        HashSet<Integer> group = new HashSet<Integer>();
        
        int[] id = new int[n];
        int[] sz = new int[n];
        for(int i = 0; i < n; i++){
            id[i] = i;
            sz[i] = 1;
            group.add(i);
        } 
        
        for(int i = 0; i < connections.length; i++){
            int x_root = find_root(id,connections[i][0]);
            int y_root = find_root(id,connections[i][1]);
            if(x_root != y_root) {
                if(sz[x_root] > sz[y_root]){
                    id[y_root] = x_root;
                    group.remove(y_root);
                }else{
                    id[x_root] = y_root;
                    group.remove(x_root);
                    sz[y_root] = Math.max(sz[y_root],sz[x_root]+1);
                }
            }
        }
        
        return group.size()-1;
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

# Solution: DFS
Refer From: [https://leetcode.com/problems/number-of-operations-to-make-network-connected/discuss/477660/Java-Count-number-of-connected-components-Clean-code](https://leetcode.com/problems/number-of-operations-to-make-network-connected/discuss/477660/Java-Count-number-of-connected-components-Clean-code)
```
class Solution {
    public int makeConnected(int n, int[][] connections) {
        if (connections.length < n - 1) return -1; // To connect all nodes need at least n-1 edges
        List<Integer>[] graph = new List[n];
        for (int i = 0; i < n; i++) graph[i] = new ArrayList<>();
        for (int[] c : connections) {
            graph[c[0]].add(c[1]);
            graph[c[1]].add(c[0]);
        }
        int components = 0;
        boolean[] visited = new boolean[n];
        for (int v = 0; v < n; v++) components += dfs(v, graph, visited);
        return components - 1; 
    }
    int dfs(int u, List<Integer>[] graph, boolean[] visited) {
        if (visited[u]) return 0;
        visited[u] = true;
        for (int v : graph[u]) dfs(v, graph, visited);
        return 1;
    }
}
```

# Solution: BFS 
Just change above dfs part to be BFS.