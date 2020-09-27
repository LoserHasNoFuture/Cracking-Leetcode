# 207_210. Course Schedule 
There are a total of  `numCourses`  courses you have to take, labeled from  `0`  to  `numCourses-1`.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair:  `[0,1]`

Given the total number of courses and a list of prerequisite  **pairs**, is it possible for you to finish all courses?

**Example 1:**

**Input:** numCourses = 2, prerequisites = [[1,0]]
**Output:** true
**Explanation:** There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0. So it is possible.

**Example 2:**

**Input:** numCourses = 2, prerequisites = [[1,0],[0,1]]
**Output:** false
**Explanation:** There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0, and to take course 0 you should
             also have finished course 1. So it is impossible.

**Constraints:**

-   The input prerequisites is a graph represented by  **a list of edges**, not adjacency matrices. Read more about  [how a graph is represented](https://www.khanacademy.org/computing/computer-science/algorithms/graph-representation/a/representing-graphs).
-   You may assume that there are no duplicate edges in the input prerequisites.
-   `1 <= numCourses <= 10^5`

# Solution: Topology Sort (207)
```
class Solution {
     public boolean canFinish(int n, int[][] prerequisites) {
        ArrayList<Integer>[] G = new ArrayList[n];
        int[] degree = new int[n];

        for (int i = 0; i < n; ++i) G[i] = new ArrayList<Integer>();
        for (int[] e : prerequisites) {
            G[e[1]].add(e[0]);
            degree[e[0]]++;
        }

        int count = 0;
        Queue<Integer> q = new LinkedList<Integer>();
        for (int i = 0; i < n; ++i)
            if (degree[i] == 0) q.add(i);
        
        while(q.size() != 0){
            int cur = q.poll();
            count++;
            for(int i: G[cur]){
                degree[i]--;
                if(degree[i] == 0) q.offer(i);
            }       
        }
        return count == n;
    }
}
```

# Solution: Topology Sort (210)
```
class Solution {
    public int[] findOrder(int n, int[][] prerequisites) {
        ArrayList<Integer>[] G = new ArrayList[n];
        int[] degree = new int[n];

        for (int i = 0; i < n; ++i) G[i] = new ArrayList<Integer>();
        for (int[] e : prerequisites) {
            G[e[1]].add(e[0]);
            degree[e[0]]++;
        }

        int count = 0;
        Queue<Integer> q = new LinkedList<Integer>();
        for (int i = 0; i < n; ++i)
            if (degree[i] == 0) q.add(i);
        
        int[] res = new int[n];
        int index = 0;
        while(q.size() != 0){
            int cur = q.poll();
            res[index++] = cur;
            count++;
            for(int i: G[cur]){
                degree[i]--;
                if(degree[i] == 0) q.offer(i);
            }       
        }
        
        return (count == n)? res: new int[0];
    }
}
```
