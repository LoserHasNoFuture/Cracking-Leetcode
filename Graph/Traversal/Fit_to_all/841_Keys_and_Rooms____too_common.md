# 841. Keys and Rooms (too common)
There are  `N`  rooms and you start in room  `0`. Each room has a distinct number in  `0, 1, 2, ..., N-1`, and each room may have some keys to access the next room.

Formally, each room  `i` has a list of keys  `rooms[i]`, and each key  `rooms[i][j]`  is an integer in  `[0, 1, ..., N-1]`  where  `N = rooms.length`. A key  `rooms[i][j] = v` opens the room with number  `v`.

Initially, all the rooms start locked (except for room  `0`).

You can walk back and forth between rooms freely.

Return  `true` if and only if you can enter every room.

**Example 1:**

**Input:** [[1],[2],[3],[]]
**Output:** true
**Explanation:** 
We start in room 0, and pick up key 1.
We then go to room 1, and pick up key 2.
We then go to room 2, and pick up key 3.
We then go to room 3.  Since we were able to go to every room, we return true.

**Example 2:**

**Input:** [[1,3],[3,0,1],[2],[0]]
**Output:** false
**Explanation:** We can't enter the room with number 2.

**Note:**

1.  `1 <= rooms.length <= 1000`
2.  `0 <= rooms[i].length <= 1000`
3.  The number of keys in all rooms combined is at most `3000`.


# Solution 1: BFS
```
class Solution {
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        int n = rooms.size();
        boolean[] visited = new boolean[n];
        
        Queue<Integer> queue = new LinkedList<Integer>();
        visited[0] = true;
        queue.offer(0);
        
        while(!queue.isEmpty()){
            int cur = queue.poll();
            
            for(int node:rooms.get(cur)){
                if(!visited[node]){
                    visited[node] = true;
                    queue.offer(node);
                }
            }
        }
        
        for(int i = 0; i < n; i++){
            if(!visited[i]) return false;
        }
        
        return true;
    }
}
```

# Solution 2: DFS Iteration
```
class Solution {
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        int n = rooms.size();
        boolean[] visited = new boolean[n];
        
        Deque<Integer> stack = new ArrayDeque<Integer>();
        visited[0] = true;
        stack.push(0);
        
        while(!stack.isEmpty()){
            int cur = stack.pop();
            
            for(int node:rooms.get(cur)){
                if(!visited[node]){
                    visited[node] = true;
                    stack.push(node);
                }
            }
        }
        
        for(int i = 0; i < n; i++){
            if(!visited[i]) return false;
        }
        
        return true;
    }
}
```

# Solution 3: DFS Recursion
```
class Solution {
    int num_of_visited = 0;
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        int n = rooms.size();
        boolean[] visited = new boolean[n];
        
        visited[0] = true;
        num_of_visited++;
        dfs(0,rooms,visited);
        
        return num_of_visited == n;
    }
    
    public void dfs(int cur, List<List<Integer>> rooms, boolean[] visited){
        for(int node: rooms.get(cur)){
            if(!visited[node]){
                num_of_visited++;
                visited[node] = true;
                dfs(node,rooms,visited);
            }
        }
    }
}
```