# 47.  Permutations II
Given a collection of numbers,  `nums`, that might contain duplicates, return  _all possible unique permutations  **in any order**._

**Example 1:**

**Input:** nums = [1,1,2]
**Output:**
[[1,1,2],
 [1,2,1],
 [2,1,1]]

**Example 2:**

**Input:** nums = [1,2,3]
**Output:** [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

**Constraints:**

-   `1 <= nums.length <= 8`
-   `-10 <= nums[i] <= 10`

# Solution 1: Back Tracking (Beat 58%)
```
class Solution {
    ArrayList<List<Integer>> res = new ArrayList<List<Integer>>();
    int n;
    public List<List<Integer>> permuteUnique(int[] nums) {
        n = nums.length;
        boolean[] visited = new boolean[n];
        
        ArrayList<Integer> cur = new ArrayList<Integer>();
        dfs(nums,-1,visited, cur);
        
        return res;
    }
    
    public void dfs(int[] nums, int index, boolean[] visited, ArrayList<Integer> cur){
        if(index >= 0) cur.add(nums[index]);
        if(cur.size() == n) res.add(new ArrayList<Integer>(cur));
        
        HashSet<Integer> set = new HashSet<Integer>();
        for(int i = 0; i < n; i++){
            if(!visited[i] && !set.contains(nums[i])){
                set.add(nums[i]);
                visited[i] = true;
                dfs(nums,i,visited,cur);
                visited[i] = false;
            }
        }
        
        if(index >= 0) cur.remove(cur.size()-1);
    }
}
```