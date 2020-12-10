# 78. Subsets
Given an integer array `nums`, return  _all possible subsets (the power set)_.

The solution set must not contain duplicate subsets.

**Example 1:**

**Input:** nums = [1,2,3]
**Output:** [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

**Example 2:**

**Input:** nums = [0]
**Output:** [[],[0]]

**Constraints:**

-   `1 <= nums.length <= 10`
-   `-10 <= nums[i] <= 10`

# Solution 1: BackTracking (Beat 56%)
```
class Solution {
    LinkedList<List<Integer>> res = new LinkedList<List<Integer>>();
    public List<List<Integer>> subsets(int[] nums) {
        ArrayList<Integer> cur = new ArrayList<Integer>();
        res.add(cur);
        
        for(int i = 0 ; i < nums.length; i++)
            dfs(nums, i, cur);
        return res;
    }
    
    public void dfs(int[] nums, int index, ArrayList<Integer> cur){
        cur.add(nums[index]);
        res.add(new ArrayList<Integer>(cur));
        
        for(int i = index+1; i < nums.length; i++) 
            dfs(nums,i,cur);
        
        cur.remove(cur.size()-1);
    }
}
```