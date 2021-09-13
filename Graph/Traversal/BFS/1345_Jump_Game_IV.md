# 1345. Jump Game IV
Given an array of integers  `arr`, you are initially positioned at the first index of the array.

In one step you can jump from index  `i`  to index:

-   `i + 1`  where: `i + 1 < arr.length`.
-   `i - 1`  where: `i - 1 >= 0`.
-   `j`  where:  `arr[i] == arr[j]`  and  `i != j`.

Return  _the minimum number of steps_  to reach the  **last index**  of the array.

Notice that you can not jump outside of the array at any time.

**Example 1:**

**Input:** arr = [100,-23,-23,404,100,23,23,23,3,404]
**Output:** 3
**Explanation:** You need three jumps from index 0 --> 4 --> 3 --> 9. Note that index 9 is the last index of the array.

**Example 2:**

**Input:** arr = [7]
**Output:** 0
**Explanation:** Start index is the last index. You don't need to jump.

**Example 3:**

**Input:** arr = [7,6,9,6,9,6,9,7]
**Output:** 1
**Explanation:** You can jump directly from index 0 to index 7 which is last index of the array.

**Example 4:**

**Input:** arr = [6,1,9]
**Output:** 2

**Example 5:**

**Input:** arr = [11,22,7,7,7,7,7,7,7,22,13]
**Output:** 3

**Constraints:**

-   `1 <= arr.length <= 5 * 104`
-   `-108  <= arr[i] <= 108`

# Solution 1: BFS
```
class Solution {
    public int minJumps(int[] nums) {
        HashMap<Integer, ArrayList<Integer>> map = new HashMap<Integer, ArrayList<Integer>>();
        int n = nums.length, steps = 0;
        if(n <= 1) return 0;
        for(int i = 0; i < n; i++){
            ArrayList<Integer> arr = map.getOrDefault(nums[i], new ArrayList<Integer>());
            arr.add(i);
            map.put(nums[i],arr);
        }
        
        Queue<Integer> queue = new LinkedList<Integer>();
        boolean[] visited = new boolean[n];
        visited[0] = true;
        queue.offer(0);
        
        while(!queue.isEmpty()){
            steps++;
            int sz = queue.size();
            
            for(int i = 0; i < sz; i++){
                int cur = queue.poll();
                
                if(cur + 1 < n && !visited[cur+1]){
                    if(cur+1 == n-1) return steps;
                    visited[cur+1] = true;
                    queue.offer(cur+1);
                }
                
                if(cur - 1 >= 0 && !visited[cur-1]){
                    visited[cur-1] = true;
                    queue.offer(cur-1);
                }
                
                if(map.containsKey(nums[cur])){
                    for(int next: map.get(nums[cur])){
                        if(visited[next]) continue;
                        if(next == n-1) return steps;
                        visited[next] = true;
                        queue.offer(next);
                    }

                    map.remove(nums[cur]);
                }
                
            }
        }
        
        return -1;
    }
}
```


# Solution 2: Two-Direction BFS
