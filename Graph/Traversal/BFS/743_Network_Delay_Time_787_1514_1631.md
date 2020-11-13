# 743. Network Delay Time 787
There are  `N`  network nodes, labelled  `1`  to  `N`.

Given  `times`, a list of travel times as  **directed**  edges  `times[i] = (u, v, w)`, where  `u`  is the source node,  `v`  is the target node, and  `w`  is the time it takes for a signal to travel from source to target.

Now, we send a signal from a certain node  `K`. How long will it take for all nodes to receive the signal? If it is impossible, return  `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png)

**Input:** times = [[2,1,1],[2,3,1],[3,4,1]], N = 4, K = 2
**Output:** 2

**Note:**

1.  `N`  will be in the range  `[1, 100]`.
2.  `K`  will be in the range  `[1, N]`.
3.  The length of  `times`  will be in the range  `[1, 6000]`.
4.  All edges  `times[i] = (u, v, w)`  will have  `1 <= u, v <= N`  and  `0 <= w <= 100`.

# Solution: BFS + PriorityQueue = Dijstra
```
class Node{
    int time;
    int id;

    Node(int id, int time){
        this.id = id;
        this.time = time;
    }
}

class Solution {
    
    public int networkDelayTime(int[][] times, int N, int k) {
        boolean[] visited = new boolean[N+1];
        int[][] dp = new int[N+1][N+1];
        parse_all_links(times,dp);
        
        int res = -1;
        PriorityQueue<Node> queue = new PriorityQueue<Node>((a,b)->(a.time-b.time));
        queue.offer(new Node(k,0));
        
        while(!queue.isEmpty()){
            Node cur = queue.poll();
            if(visited[cur.id]) continue;
            visited[cur.id] = true;
            res = cur.time;
            
            for(int i = 1; i < N+1; i++){
                if(!visited[i] && dp[cur.id][i] >= 0){
                    queue.offer(new Node(i,cur.time+dp[cur.id][i]));
                }
            }
        }
        
        for(int i = 1; i < N+1; i++){
            if(!visited[i]) return -1;
        }
        return res;
    }
    
    public void parse_all_links(int[][] links, int[][] dp){
        for(int i = 0; i < dp.length; i++){
             Arrays.fill(dp[i],-1);
        }

        for(int i = 0; i < links.length; i++){
            dp[links[i][0]][links[i][1]] = links[i][2];
        }
    }
}
```