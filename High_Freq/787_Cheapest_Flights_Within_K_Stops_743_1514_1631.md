# 787. Cheapest Flights Within K Stops 743
There are  `n`  cities connected by `m`  flights. Each flight starts from city `u`  and arrives at `v`  with a price  `w`.

Now given all the cities and flights, together with starting city  `src`  and the destination `dst`, your task is to find the cheapest price from  `src`  to  `dst`  with up to  `k`  stops. If there is no such route, output  `-1`.

**Example 1:**
**Input:** 
n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
src = 0, dst = 2, k = 1
**Output:** 200
**Explanation:** 
The graph looks like this:
![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png)

The cheapest price from city `0` to city `2` with at most 1 stop costs 200, as marked red in the picture.

**Example 2:**
**Input:** 
n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
src = 0, dst = 2, k = 0
**Output:** 500
**Explanation:** 
The graph looks like this:
![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png)

The cheapest price from city `0` to city `2` with at most 0 stop costs 500, as marked blue in the picture.

**Constraints:**

-   The number of nodes `n`  will be in range  `[1, 100]`, with nodes labeled from  `0`  to  `n` `- 1`.
-   The size of  `flights`  will be in range  `[0, n * (n - 1) / 2]`.
-   The format of each flight will be  `(src,` `dst``, price)`.
-   The price of each flight will be in the range  `[1, 10000]`.
-   `k`  is in the range of  `[0, n - 1]`.
-   There will not be any duplicated flights or self cycles.

# Solution 1: BFS + PriorityQueue == Modified Dijistra 
Note that the number of stops would affect results:
For some routes, it exceeds stops limit but has smaller cost.


```
class Solution {
    
    class Node implements Comparable<Node>{
        int id;
        int delay;
        int stop;
        
        public Node(int _id, int _delay, int _stop){
            this.id = _id;
            this.delay = _delay;
            this.stop = _stop;
        }
        
        @Override
        public int compareTo(Node that){
            return this.delay - that.delay;
        }
    }
    
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        int[][] mat = new int[n][n];
        parseAllFlights(n,mat,flights);
        
        boolean[] visited = new boolean[n];
        int[] stop = new int[n];
        Arrays.fill(stop,n);
        
        PriorityQueue<Node> pq = new PriorityQueue<Node>();
        pq.offer(new Node(src, 0, -1));
        
        while(!pq.isEmpty()){
            Node cur = pq.poll();
            if(cur.id == dst) return cur.delay;
            visited[cur.id] = true;
            stop[cur.id] = cur.stop;
            
            for(int i = 0; i < n; i++){
                if(mat[cur.id][i] == -1 || cur.stop + 1 > k) continue;
                if(visited[i] && cur.stop + 1 >= stop[i]) continue;
                pq.offer(new Node(i,cur.delay+mat[cur.id][i], cur.stop+1));
            }
        }
        
        return -1;
    }
    
    public void parseAllFlights(int n, int[][] mat, int[][] flights){
        for(int i = 0; i < n; i++){
            Arrays.fill(mat[i],-1);
            mat[i][i] = 0;
        }
        
        for(int[] flight: flights){
            mat[flight[0]][flight[1]] = flight[2];
        }
    }
}
```
