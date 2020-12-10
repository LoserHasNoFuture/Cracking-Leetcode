# 90. Subsets II
Given a collection of integers that might contain duplicates,  **_nums_**, return all possible subsets (the power set).

**Note:**  The solution set must not contain duplicate subsets.

**Example:**

**Input:** [1,2,2]
**Output:**
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]

# Solution 1: BackTracking (Beat 99%)
```
class Solution {
    LinkedList<List<Integer>> res = new LinkedList<List<Integer>>();
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        ArrayList<Integer> cur = new ArrayList<Integer>();
        res.add(cur);
        
        Arrays.sort(nums);
        dfs(nums, -1, cur);
        
        return res;
    }
    
    public void dfs(int[] nums, int index, ArrayList<Integer> cur){
        if(index >= 0){
            cur.add(nums[index]);
            res.add(new ArrayList<Integer>(cur));    
        }
        
        for(int i = index+1; i < nums.length; i++) {
            if(i > index + 1 && nums[i] == nums[i-1]) continue;
            dfs(nums,i,cur);
        }
        
        if(index >= 0) cur.remove(cur.size()-1);
    }
}
```