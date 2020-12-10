# 46. Permutations
Given an array  `nums`  of distinct integers, return  _all the possible permutations_. You can return the answer in  **any order**.

**Example 1:**

**Input:** nums = [1,2,3]
**Output:** [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

**Example 2:**

**Input:** nums = [0,1]
**Output:** [[0,1],[1,0]]

**Example 3:**

**Input:** nums = [1]
**Output:** [[1]]

**Constraints:**

-   `1 <= nums.length <= 6`
-   `-10 <= nums[i] <= 10`
-   All the integers of  `nums`  are  **unique**.

# Solution 1: BackTracking (Recursion) Beat 92%
```
class Solution {
    ArrayList<List<Integer>> res = new ArrayList<List<Integer>>();
    int n;
    public List<List<Integer>> permute(int[] nums) {
        n = nums.length;
        boolean[] visited = new boolean[n];
        
        ArrayList<Integer> cur = new ArrayList<Integer>();
        dfs(nums,-1,visited, cur);
        
        return res;
    }
    
    public void dfs(int[] nums, int index, boolean[] visited, ArrayList<Integer> cur){
        if(index >= 0) cur.add(nums[index]);
        if(cur.size() == n) res.add(new ArrayList<Integer>(cur));
        
        for(int i = 0; i < n; i++){
            if(!visited[i]){
                visited[i] = true;
                dfs(nums,i,visited,cur);
                visited[i] = false;
            }
        }
        
        if(index >= 0) cur.remove(cur.size()-1);
    }
} 
```