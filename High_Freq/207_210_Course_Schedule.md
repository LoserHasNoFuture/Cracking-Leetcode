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
    public boolean canFinish(int n, int[][] prereqs) {
        int[] in_degree = new int[n];
        List<Integer>[] outpath = new List[n];
        for(int i = 0; i < n; i++) outpath[i] = new ArrayList<Integer>();
        
        for(int[] pre: prereqs){
            in_degree[pre[1]]++;
            outpath[pre[0]].add(pre[1]);
        }
        
        int cnt = 0;
        Queue<Integer> queue = new LinkedList<Integer>();
        for(int i = 0; i < in_degree.length; i++){
            if(in_degree[i] == 0) queue.offer(i);
        }
        
        
        while(!queue.isEmpty()){
            if(queue.size() + cnt == n) return true;
            
            cnt++;
            int course = queue.poll();
            
            for(int num: outpath[course]){
                in_degree[num]--;
                if(in_degree[num] == 0) queue.offer(num);
            }
            
        }
        
        return cnt == n;
    }
    
}
```

# Solution: Topology Sort (210)
```
class Solution {
    public int[] findOrder(int n, int[][] prereqs) {
        int[] in_degree = new int[n], res = new int[n];
        List<Integer>[] outpath = new List[n];
        for(int i = 0; i < n; i++) outpath[i] = new ArrayList<Integer>();
        
        for(int[] pre: prereqs){
            in_degree[pre[0]]++;
            outpath[pre[1]].add(pre[0]);
        }
        
        int cnt = 0, index = 0;
        Queue<Integer> queue = new LinkedList<Integer>();
        for(int i = 0; i < in_degree.length; i++){
            if(in_degree[i] == 0) queue.offer(i);
        }
        
        
        while(!queue.isEmpty()){
            if(queue.size() + cnt == n) {
                
                while(!queue.isEmpty()){
                    res[index++] = queue.poll();
                }
                
                return res;
            }
            
            cnt++;
            int course = queue.poll();
            res[index++] = course;
            
            for(int num: outpath[course]){
                in_degree[num]--;
                if(in_degree[num] == 0) queue.offer(num);
            }
            
        }
        
        return cnt == n?res:new int[0];
    }
}


```
