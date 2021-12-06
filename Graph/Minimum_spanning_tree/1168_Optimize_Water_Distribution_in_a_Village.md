# 1168. Optimize Water Distribution in a Village
There are  `n`  houses in a village. We want to supply water for all the houses by building wells and laying pipes.

For each house  `i`, we can either build a well inside it directly with cost  `wells[i - 1]`  (note the  `-1`  due to  **0-indexing**), or pipe in water from another well to it. The costs to lay pipes between houses are given by the array  `pipes`, where each  `pipes[j] = [house1j, house2j, costj]`  represents the cost to connect  `house1j`  and  `house2j`  together using a pipe. Connections are bidirectional.

Return  _the minimum total cost to supply water to all houses_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/05/22/1359_ex1.png)

**Input:** n = 3, wells = [1,2,2], pipes = [[1,2,1],[2,3,1]]
**Output:** 3
**Explanation:** 
The image shows the costs of connecting houses using pipes.
The best strategy is to build a well in the first house with cost 1 and connect the other houses to it with cost 2 so the total cost is 3.

**Example 2:**

**Input:** n = 2, wells = [1,1], pipes = [[1,2,1]]
**Output:** 2

**Constraints:**

-   `2 <= n <= 104`
-   `wells.length == n`
-   `0 <= wells[i] <= 105`
-   `1 <= pipes.length <= 104`
-   `pipes[j].length == 3`
-   `1 <= house1j, house2j  <= n`
-   `0 <= costj  <= 105`
-   `house1j  != house2j`

# Solution:
```
class Solution {
    public int minCostToSupplyWater(int n, int[] wells, int[][] pipes) {
        HashMap<Integer,List<int[]>> map = new HashMap<Integer,List<int[]>>();
        List<int[]> arr = new ArrayList<int[]>();
        int res = 0;
        for(int i = 0; i < n; i++) arr.add(new int[]{i+1,wells[i]});
        map.put(0,arr);
        
        for(int[] pipe: pipes){
            arr = map.getOrDefault(pipe[0], new ArrayList<int[]>());
            arr.add(new int[]{pipe[1], pipe[2]});
            map.put(pipe[0],arr);
            
            arr = map.getOrDefault(pipe[1], new ArrayList<int[]>());
            arr.add(new int[]{pipe[0], pipe[2]});
            map.put(pipe[1],arr);
        }
        
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>((a,b)->Integer.compare(a[1],b[1]));
        pq.offer(new int[]{0,0});
        boolean[] visited = new boolean[++n];
        
        while(!pq.isEmpty() && n > 0){
            int[] cur = pq.poll();
            if(visited[cur[0]]) continue;
            n--; visited[cur[0]] = true;
            res += cur[1];
            
            if(!map.containsKey(cur[0])) continue;
            for(int[] pipe: map.get(cur[0])){
                if(visited[pipe[0]]) continue;
                pq.offer(new int[]{pipe[0], pipe[1]});
            }
        }
        
        return res;
    }
}
```