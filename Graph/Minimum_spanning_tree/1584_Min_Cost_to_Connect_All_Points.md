# 1584. Min Cost to Connect All Points
You are given an array `points` representing integer coordinates of some points on a 2D-plane, where  `points[i] = [xi, yi]`.

The cost of connecting two points  `[xi, yi]`  and  `[xj, yj]`  is the  **manhattan distance**  between them: `|xi  - xj| + |yi  - yj|`, where  `|val|`  denotes the absolute value of `val`.

Return _the minimum cost to make all points connected._  All points are connected if there is  **exactly one**  simple path between any two points.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/26/d.png)

**Input:** points = [[0,0],[2,2],[3,10],[5,2],[7,0]]
**Output:** 20
**Explanation:** ![](https://assets.leetcode.com/uploads/2020/08/26/c.png)
We can connect the points as shown above to get the minimum cost of 20.
Notice that there is a unique path between every pair of points.

**Example 2:**

**Input:** points = [[3,12],[-2,5],[-4,1]]
**Output:** 18

**Example 3:**

**Input:** points = [[0,0],[1,1],[1,0],[-1,1]]
**Output:** 4

**Example 4:**

**Input:** points = [[-1000000,-1000000],[1000000,1000000]]
**Output:** 4000000

**Example 5:**

**Input:** points = [[0,0]]
**Output:** 0

**Constraints:**

-   `1 <= points.length <= 1000`
-   `-106 <= xi, yi  <= 106`
-   All pairs  `(xi, yi)`  are distinct.

# Solution
```
class Solution {
    public int minCostConnectPoints(int[][] points) {
        // a[0] is index of point
        // a[1] is distance
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>((a,b)->Integer.compare(a[1],b[1]));
        int n = points.length, res = 0;
        boolean[] visited = new boolean[n];
        
        pq.offer(new int[]{0,0});
        while(!pq.isEmpty() && n > 0){
            int[] cur = pq.poll();
            if(visited[cur[0]]) continue;
            res += cur[1];
            n--; visited[cur[0]] = true;
            
            for(int i = 0; i < points.length; i++){
                if(i == cur[0]) continue;
                int distance = Math.abs(points[cur[0]][0] - points[i][0]) + 
                    Math.abs(points[cur[0]][1] - points[i][1]);
                pq.offer(new int[]{i,distance});
            }
        }
        
        return res;
    }
}
```
