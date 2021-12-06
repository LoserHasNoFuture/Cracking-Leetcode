# 1135. Connecting Cities With Minimum Cost
There are  `N`  cities numbered from 1 to  `N`.

You are given  `connections`, where each  `connections[i] = [city1, city2, cost]` represents the cost to connect  `city1`  and  `city2`  together. (A  _connection_  is bidirectional: connecting  `city1`  and  `city2`  is the same as connecting  `city2`  and  `city1`.)

Return the minimum cost so that for every pair of cities, there exists a path of connections (possibly of length 1) that connects those two cities together. The cost is the sum of the connection costs used. If the task is impossible, return -1.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/04/20/1314_ex2.png)

**Input:** N = 3, connections = [[1,2,5],[1,3,6],[2,3,1]]
**Output:** 6
**Explanation:** 
Choosing any 2 edges will connect all cities so we choose the minimum 2.

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/04/20/1314_ex1.png)

**Input:** N = 4, connections = [[1,2,3],[3,4,4]]
**Output:** -1
**Explanation:** 
There is no way to connect all cities even if all edges are used.

**Note:**

1.  `1 <= N <= 10000`
2.  `1 <= connections.length <= 10000`
3.  `1 <= connections[i][0], connections[i][1] <= N`
4.  `0 <= connections[i][2] <= 10^5`
5.  `connections[i][0] != connections[i][1]`

# Solution 1: Prim's Algorithm (Beat 20%)
This is a minimal spanning tree problem.
The key of this algorithm is:
Suppose we have **n** vertices in total, and we divide them into two groups.
One group has **m** vertices, and another has **n-m** vertices.
The edge who links two groups and has minial cost, must exist in the minimal spanning tree.

```
class Solution {
    class Edge{
        int u;
        int v;
        int cost;
        
        Edge(int u, int v, int cost){
            this.u = u;
            this.v = v;
            this.cost = cost;
        }
        
    }
    
    public int minimumCost(int N, int[][] connections) {
        if(N == 1) return 0;
        if(connections.length == 0) return -1;
        int res = 0;
        
        HashMap<Integer, ArrayList<Edge>> map = new HashMap<Integer, ArrayList<Edge>>();
        for(int i = 1; i <= N; i++) map.put(i, new ArrayList<Edge>());
        parse_all_edges(map,connections);
        
        PriorityQueue<Edge> pq = new PriorityQueue<Edge>((a,b)->a.cost-b.cost);
        boolean[] visited = new boolean[N+1];
        
        int num_of_visited = 1;
        visited[1] = true;
        
        for(Edge edge: map.get(1)) pq.offer(edge);
        
        while(!pq.isEmpty()){
            Edge cur = pq.poll();
            int next_node = -1;
            if(visited[cur.u] && visited[cur.v]) continue;
            res += cur.cost;
            
            if(visited[cur.u]) next_node = cur.v;
            else next_node = cur.u;
            
            visited[next_node] = true;
            if(++num_of_visited == N) return res;
            
            for(Edge edge: map.get(next_node)) pq.offer(edge);
            
        }
        
        
        return -1;
    }
    
    public void parse_all_edges(HashMap<Integer, ArrayList<Edge>> map, int[][] connections){
        ArrayList<Edge> arr;
        for(int[] conn:connections){
            Edge edge = new Edge(conn[0],conn[1],conn[2]);
            arr = map.get(conn[0]);
            arr.add(edge);
            map.put(conn[0],arr);
            
            arr = map.get(conn[1]);
            arr.add(edge);
            map.put(conn[1],arr);
        }
    }
}
```


# Solution 2: Kruskal's Algorithm (Union Find)
Sort all edges first, and applying union find.

```
class Solution {

    public int minimumCost(int N, int[][] connections) {
        if(N == 1) return 0;
        if(connections.length == 0) return -1;
        int res = 0;
        
        int[] id = new int[N+1];
        int[] sz = new int[N+1];
        for(int i = 1; i <= N; i++) id[i] = i;
        
        Arrays.sort(connections, (a,b)->(a[2]-b[2]));
        for(int[] conn:connections) {
            int root1 = find_root(id,conn[0]);
            int root2 = find_root(id,conn[1]);
            
            if(root1 == root2) continue;
            else{
                res += conn[2];   
                if(sz[root1] > sz[root2]) id[root2] = root1;
                 else{
                    id[root1] = root2;
                    sz[root1] = Math.max(sz[root1],sz[root2] + 1);
                }
            }
            
        }
        
        
        int root = find_root(id, 1);
        for(int i = 2; i <= N; i++) if(find_root(id,i) != root) return -1;
        
        return res;
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