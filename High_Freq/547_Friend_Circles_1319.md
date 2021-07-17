# 547. Friend Circles 1319
There are  **N**  students in a class. Some of them are friends, while some are not. Their friendship is transitive in nature. For example, if A is a  **direct**  friend of B, and B is a  **direct**  friend of C, then A is an  **indirect**  friend of C. And we defined a friend circle is a group of students who are direct or indirect friends.

Given a  **N*N**  matrix  **M**  representing the friend relationship between students in the class. If M[i][j] = 1, then the ith  and jth  students are  **direct**  friends with each other, otherwise not. And you have to output the total number of friend circles among all the students.

**Example 1:**

**Input:** 
[[1,1,0],
 [1,1,0],
 [0,0,1]]
**Output:** 2
**Explanation:**The 0th and 1st students are direct friends, so they are in a friend circle. 
The 2nd student himself is in a friend circle. So return 2.

**Example 2:**

**Input:** 
[[1,1,0],
 [1,1,1],
 [0,1,1]]
**Output:** 1
**Explanation:**The 0th and 1st students are direct friends, the 1st and 2nd students are direct friends, 
so the 0th and 2nd students are indirect friends. All of them are in the same friend circle, so return 1.

**Constraints:**

-   `1 <= N <= 200`
-   `M[i][i] == 1`
-   `M[i][j] == M[j][i]`

# Solution 1: DFS
```
class Solution {
    public int findCircleNum(int[][] M) {
        int n = M.length;
        if(n <= 1) return n;
        
        boolean[] visited = new boolean[n];
        int count = 0;
        for(int i = 0; i < n; i++){
            if(!visited[i]){
                dfs(M,visited,i);
                count++;
            }
        }
        
        return count;
    }
    
    // recursion
    public void dfs(int[][] M, boolean[] visited, int i){
        if(visited[i]) return;
        visited[i] = true;
        
        for(int j = 1; j < visited.length; j++){
            if(M[i][j] == 1){
                dfs(M,visited,j);
            }
        }
    }

	// iteration
	public void dfs(int[][] M, boolean[] visited, int i){
        Deque<Integer> stack = new ArrayDeque<>();
        visited[i] = true;
        stack.push(i);
        
        while(!stack.isEmpty()){
            int cur = stack.pop();
            for(int j = 0; j < visited.length; j++){
                if(M[cur][j] == 1 && !visited[j]) {
                    visited[j] = true;
                    stack.push(j);
                }
            }
        }
        return;
    }
}
```

# Solution 2: Union Find
```
class Solution {
    public int findCircleNum(int[][] M) {
        int n = M.length;
        if(n <= 1) return n;
        
        int[] id = new int[n];
        int[] sz = new int[n];
        for(int i = 0; i < n; i++) id[i] = i;
        
        for(int i = 0; i < n; i++){
            for(int j = i+1; j < n; j++){
                if(M[i][j] == 1){
                    int x = find_root(id,i);
                    int y = find_root(id,j);
                    if(x != y){
                        if(sz[x] > sz[y]){
                            id[y] = x;
                        }else{
                            id[x] = y;
                            sz[y] = Math.max(sz[y],sz[x]+1);
                        }
                    }
                }
            }
        }
        
        HashSet<Integer> set = new HashSet<Integer>();
        for(int i = 0; i < n; i++) set.add(find_root(id,i));
        
        return set.size();
        
    }
    
    public int find_root(int[] id, int i){
        while(id[i] != i){
            id[i] = id[id[i]];
            i = id[i];
        }
        return i;
    }
}
```

# Solution 3: BFS
```
class Solution {
    public int findCircleNum(int[][] M) {
        int n = M.length;
        if(n <= 1) return n;
        
        boolean[] visited = new boolean[n];
        int count = 0;
        for(int i = 0; i < n; i++){
            if(!visited[i]){
                count++;
                bfs(M,visited,i);
            }
        }
        
        return count;
    }
    
    
    public void bfs(int[][] M, boolean[] visited, int i){
        Queue<Integer> queue = new LinkedList<>();
        visited[i] = true;
        queue.offer(i);
        
        while(!queue.isEmpty()){
            int cur = queue.poll();
            for(int j = 0; j < visited.length; j++){
                if(M[cur][j] == 1 && !visited[j]) {
                    visited[j] = true;
                    queue.offer(j);
                }
            }
        }
        return;
    }

}
```
