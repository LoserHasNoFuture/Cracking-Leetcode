# 1466. Reorder Routes to Make All Paths Lead to the City Zero
There are `n` cities numbered from `0` to `n-1`  and `n-1`  roads such that there is only one way to travel between two different cities (this network form a tree). Last year, The ministry of transport decided to orient the roads in one direction because they are too narrow.

Roads are represented by `connections` where `connections[i] = [a, b]` represents a road from city `a` to `b`.

This year, there will be a big event in the capital (city 0), and many people want to travel to this city.

Your task consists of reorienting some roads such that each city can visit the city 0. Return the  **minimum**  number of edges changed.

It's  **guaranteed**  that each city can reach the city 0 after reorder.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2020/05/13/sample_1_1819.png)**

**Input:** n = 6, connections = [[0,1],[1,3],[2,3],[4,0],[4,5]]
**Output:** 3
**Explanation:** Change the direction of edges show in red such that each node can reach the node 0 (capital).

**Example 2:**

**![](https://assets.leetcode.com/uploads/2020/05/13/sample_2_1819.png)**

**Input:** n = 5, connections = [[1,0],[1,2],[3,2],[3,4]]
**Output:** 2
**Explanation:** Change the direction of edges show in red such that each node can reach the node 0 (capital).

**Example 3:**

**Input:** n = 3, connections = [[1,0],[2,0]]
**Output:** 0

**Constraints:**

-   `2 <= n <= 5 * 10^4`
-   `connections.length == n-1`
-   `connections[i].length == 2`
-   `0 <= connections[i][0], connections[i][1] <= n-1`
-   `connections[i][0] != connections[i][1]`


# Solution 1: DFS (Beat 67%)
```
class Solution {
    
    boolean[] visited;
    public int minReorder(int n, int[][] connections) {
        ArrayList<Integer>[] in = new ArrayList[n];
        ArrayList<Integer>[] out = new ArrayList[n];
        visited = new boolean[n];
        
        for(int i = 0; i < n; i++){
            in[i] = new ArrayList<Integer>();
            out[i] = new ArrayList<Integer>();
        }
        
        for(int[] conn:connections){
            in[conn[1]].add(conn[0]);
            out[conn[0]].add(conn[1]);
        }
        
        visited[0] = true;
        return dfs(in, out,0);
    }
    
    public int dfs(ArrayList<Integer>[] in, ArrayList<Integer>[] out, int index){
        int cnt = 0;
        
        for(int next: out[index]){
            if(!visited[next]){
                cnt++;
                visited[next] = true;
                cnt += dfs(in,out,next);
            }
        }
        
        for(int next: in[index]){
            if(!visited[next]){
                visited[next] = true;
                cnt += dfs(in,out,next);
            }
        }
        
        return cnt;
    }
}
```

Better Coding Style:
Refer From: [https://leetcode.com/problems/reorder-routes-to-make-all-paths-lead-to-the-city-zero/discuss/661672/C%2B%2BJava-Track-Direction](https://leetcode.com/problems/reorder-routes-to-make-all-paths-lead-to-the-city-zero/discuss/661672/C%2B%2BJava-Track-Direction)
```
int dfs(List<List<Integer>> al, boolean[] visited, int from) {
    int change = 0;
    visited[from] = true;
    for (var to : al.get(from))
        if (!visited[Math.abs(to)])
            change += dfs(al, visited, Math.abs(to)) + (to > 0 ? 1 : 0);
    return change;   
}
public int minReorder(int n, int[][] connections) {
    List<List<Integer>> al = new ArrayList<>();
    for(int i = 0; i < n; ++i) 
        al.add(new ArrayList<>());
    for (var c : connections) {
        al.get(c[0]).add(c[1]);
        al.get(c[1]).add(-c[0]);
    }
    return dfs(al, new boolean[n], 0);
}
```


# Solution 2: BFS (Beat 69%)
```
class Solution {
    
    
    public int minReorder(int n, int[][] connections) {
        ArrayList<Integer>[] links = new ArrayList[n];
        
        
        boolean[] visited = new boolean[n];
        
        for(int i = 0; i < n; i++) links[i] = new ArrayList<Integer>();
        
        
        for(int[] conn:connections){
            links[conn[1]].add(conn[0]);
            links[conn[0]].add(-conn[1]);
        }
        
        visited[0] = true;
        int res = 0;
        
        Queue<Integer> queue = new LinkedList<Integer>();
        queue.offer(0);
        
        while(!queue.isEmpty()){
            int cur = queue.poll();
            
            for(int next: links[cur]){
                if(!visited[Math.abs(next)]){
                    visited[Math.abs(next)] = true;
                    if(next < 0) res++;
                    queue.offer(Math.abs(next));
                }
            }
        }
        
        return res;
    }
}
```