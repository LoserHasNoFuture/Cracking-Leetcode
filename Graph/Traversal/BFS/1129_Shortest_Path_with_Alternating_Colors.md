# 1129. Shortest Path with Alternating Colors
Consider a directed graph, with nodes labelled  `0, 1, ..., n-1`. In this graph, each edge is either red or blue, and there could be self-edges or parallel edges.

Each  `[i, j]`  in  `red_edges`  denotes a red directed edge from node  `i`  to node  `j`. Similarly, each  `[i, j]`  in  `blue_edges`  denotes a blue directed edge from node  `i`  to node  `j`.

Return an array  `answer` of length  `n`, where each `answer[X]` is the length of the shortest path from node  `0` to node  `X` such that the edge colors alternate along the path (or  `-1`  if such a path doesn't exist).

**Example 1:**

**Input:** n = 3, red_edges = [[0,1],[1,2]], blue_edges = []
**Output:** [0,1,-1]

**Example 2:**

**Input:** n = 3, red_edges = [[0,1]], blue_edges = [[2,1]]
**Output:** [0,1,-1]

**Example 3:**

**Input:** n = 3, red_edges = [[1,0]], blue_edges = [[2,1]]
**Output:** [0,-1,-1]

**Example 4:**

**Input:** n = 3, red_edges = [[0,1]], blue_edges = [[1,2]]
**Output:** [0,1,2]

**Example 5:**

**Input:** n = 3, red_edges = [[0,1],[0,2]], blue_edges = [[1,0]]
**Output:** [0,1,1]

**Constraints:**

-   `1 <= n <= 100`
-   `red_edges.length <= 400`
-   `blue_edges.length <= 400`
-   `red_edges[i].length == blue_edges[i].length == 2`
-   `0 <= red_edges[i][j], blue_edges[i][j] < n`


# Solution 1: BFS ith one queue
```
class Solution {
    
    class Node {
        int dis;
        int id;
        int color;
        Node(int id, int dis, int color){
            this.id = id;
            this.dis = dis;
            this.color = color;
        }
    }
    
    public int[] shortestAlternatingPaths(int n, int[][] red_edges, int[][] blue_edges) {
        int[] res = new int[n];
        int[] visited = new int[n];
        visited[0] = 3;
        for(int i = 1; i < n; i++) res[i] = -1;
        
        HashMap<Integer, ArrayList<Integer>> rmap = new HashMap<Integer, ArrayList<Integer>>();
        HashMap<Integer, ArrayList<Integer>> bmap = new HashMap<Integer, ArrayList<Integer>>();
        
        parse_edges_to_map(red_edges,rmap,n);
        parse_edges_to_map(blue_edges,bmap,n);
        
        Queue<Node> queue = new LinkedList<Node>();
        
        Node cur = new Node(0,0,1);
        queue.offer(cur);
        cur = new Node(0,0,2);
        queue.offer(cur);
        
        while(!queue.isEmpty()){
            int size = queue.size();
            
            for(int i = 0; i < size; i++){
                cur = queue.poll();
                if((cur.color&1) == 0){
                    for(int node: bmap.get(cur.id)){
                        if((visited[node]&1) == 0){
                            res[node] = res[node] == -1? cur.dis + 1 : Math.min(cur.dis+1,res[node]);
                            queue.offer(new Node(node,cur.dis+1,1));    
                            visited[node]++;
                        }
                    }
                }
                
                if((cur.color&2) == 0){
                    for(int node: rmap.get(cur.id)){
                        if((visited[node]&2) == 0){
                            res[node] = res[node] == -1? cur.dis + 1 : Math.min(cur.dis+1,res[node]);
                            queue.offer(new Node(node,cur.dis+1,2));    
                            visited[node] = visited[node] + 2;
                        }
                    }    
                }
                
            }
        }
        
        return res;
    }
    
    public void parse_edges_to_map(int[][] edges, HashMap<Integer, ArrayList<Integer>> map, int n){
        ArrayList<Integer> arr;
        for(int i = 0; i < n; i++){
            arr = new ArrayList<Integer>();
            map.put(i,arr);
        }
        
        for(int[] edge:edges){
            arr = map.get(edge[0]);
            arr.add(edge[1]);
            map.put(edge[0],arr);
        }
    }
}
```

# Solution 2: BFS ith two queues
```
class Solution {
    
    class Node {
        int dis;
        int id;
        Node(int id, int dis){
            this.id = id;
            this.dis = dis;
        }
    }
    
    public int[] shortestAlternatingPaths(int n, int[][] red_edges, int[][] blue_edges) {
        int[] res = new int[n];
        int[] visited = new int[n];
        visited[0] = 3;
        for(int i = 1; i < n; i++) res[i] = -1;
        
        HashMap<Integer, ArrayList<Integer>> rmap = new HashMap<Integer, ArrayList<Integer>>();
        HashMap<Integer, ArrayList<Integer>> bmap = new HashMap<Integer, ArrayList<Integer>>();
        
        parse_edges_to_map(red_edges,rmap,n);
        parse_edges_to_map(blue_edges,bmap,n);
        
        Queue<Node> redQ = new LinkedList<Node>();
        Queue<Node> blueQ = new LinkedList<Node>();
        
        Node cur = new Node(0,0);
        redQ.offer(cur);
        blueQ.offer(cur);
        
        while(!blueQ.isEmpty() || !redQ.isEmpty()){
            int rsize = redQ.size();
            int bsize = blueQ.size();
            
            for(int i = 0; i < rsize; i++){
                cur = redQ.poll();
                for(int node: bmap.get(cur.id)){
                    if((visited[node]&1) == 0){
                        res[node] = res[node] == -1? cur.dis + 1 : Math.min(cur.dis+1,res[node]);
                        blueQ.offer(new Node(node,cur.dis+1));    
                        visited[node]++;
                    }
                    
                }
            }
            
            for(int j = 0; j < bsize; j++){
                cur = blueQ.poll();
                for(int node: rmap.get(cur.id)){
                    if((visited[node]&2) == 0){
                        res[node] = res[node] == -1? cur.dis + 1 : Math.min(cur.dis+1,res[node]);
                        redQ.offer(new Node(node,cur.dis+1));
                        visited[node] = visited[node]+2;
                    }
                }
            }
        }
        
        return res;
    }
    
    public void parse_edges_to_map(int[][] edges, HashMap<Integer, ArrayList<Integer>> map, int n){
        ArrayList<Integer> arr;
        for(int i = 0; i < n; i++){
            arr = new ArrayList<Integer>();
            map.put(i,arr);
        }
        
        for(int[] edge:edges){
            arr = map.get(edge[0]);
            arr.add(edge[1]);
            map.put(edge[0],arr);
        }
    }
}
```