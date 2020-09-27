# 1306. Jump Game III
Given an array of non-negative integers  `arr`, you are initially positioned at  `start` index of the array. When you are at index  `i`, you can jump to  `i + arr[i]`  or  `i - arr[i]`, check if you can reach to  **any**  index with value 0.

Notice that you can not jump outside of the array at any time.

**Example 1:**

**Input:** arr = [4,2,3,0,3,1,2], start = 5
**Output:** true
**Explanation:** 
All possible ways to reach at index 3 with value 0 are: 
index 5 -> index 4 -> index 1 -> index 3 
index 5 -> index 6 -> index 4 -> index 1 -> index 3 

**Example 2:**

**Input:** arr = [4,2,3,0,3,1,2], start = 0
**Output:** true 
**Explanation:** One possible way to reach at index 3 with value 0 is: 
index 0 -> index 4 -> index 1 -> index 3

**Example 3:**

**Input:** arr = [3,0,2,1,2], start = 2
**Output:** false
**Explanation:** There is no way to reach at index 1 with value 0.

**Constraints:**

-   `1 <= arr.length <= 5 * 10^4`
-   `0 <= arr[i] < arr.length`
-   `0 <= start < arr.length`

# Solution 1: DFS Recursion
```
class Solution {
    boolean[] visited;
    public boolean canReach(int[] arr, int start) {
        visited = new boolean[arr.length];
        visited[start] = true;
        return dfs(arr,start);
    }
    
    public boolean dfs(int[] arr, int start){
        if(arr[start] == 0) return true;
        
        if(start - arr[start] >= 0 && !visited[start - arr[start]]){
            visited[start - arr[start]] = true;
            if(dfs(arr,start - arr[start])) return true;
        }
        
        if(start + arr[start] < arr.length && !visited[start + arr[start]]){
            visited[start + arr[start]] = true;
            if(dfs(arr,start + arr[start])) return true;
        }
        
        return false;
    } 
}
```

# Solution 2: DFS Iteration
```
class Solution {
    
    public boolean canReach(int[] arr, int start) {
        
        boolean[] visited = new boolean[arr.length];
        visited[start] = true;

        Deque<Integer> stack = new ArrayDeque<>();
        stack.push(start);
        
        while(!stack.isEmpty()){
            int cur = stack.pop();
            if(arr[cur] == 0) return true;
            if(cur - arr[cur] >= 0 && !visited[cur - arr[cur]]){
                visited[cur - arr[cur]] = true;
                stack.push(cur - arr[cur]);
            }
            
            if(cur + arr[cur] < arr.length && !visited[cur + arr[cur]]){
                visited[cur + arr[cur]] = true;
                stack.push(cur + arr[cur]);
            }
        }
        
        return false;
    }
}
```

# Solution 3: BFS
```
class Solution {
    public boolean canReach(int[] arr, int start) {
        boolean[] visited = new boolean[arr.length];
        Queue<Integer> q = new LinkedList<Integer>();
        
        q.offer(start);
        visited[start] = true;
        
        while(!q.isEmpty()){
            int cur = q.poll();
            if(arr[cur] == 0) return true;
            if(cur - arr[cur] >= 0 && 
               !visited[cur - arr[cur]]) {
                q.offer(cur - arr[cur]);
                visited[cur - arr[cur]] = true;
            }
            if(cur + arr[cur] < arr.length && 
               !visited[cur + arr[cur]]) {
                q.offer(cur + arr[cur]);
                visited[cur + arr[cur]] = true;                
            }
        }
        
        return false;
    }
}
```