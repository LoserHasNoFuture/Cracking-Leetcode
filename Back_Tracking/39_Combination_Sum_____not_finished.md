# 39. Combination Sum  
Given an array of  **distinct**  integers  `candidates`  and a target integer  `target`, return  _a list of all  **unique combinations**  of_ `candidates` _where the chosen numbers sum to_ `target`_._  You may return the combinations in  **any order**.

The  **same**  number may be chosen from  `candidates`  an  **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

**Example 1:**

**Input:** candidates = [2,3,6,7], target = 7
**Output:** [[2,2,3],[7]]
**Explanation:**
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.

**Example 2:**

**Input:** candidates = [2,3,5], target = 8
**Output:** [[2,2,2,2],[2,3,3],[3,5]]

**Example 3:**

**Input:** candidates = [2], target = 1
**Output:** []

**Example 4:**

**Input:** candidates = [1], target = 1
**Output:** [[1]]

**Example 5:**

**Input:** candidates = [1], target = 2
**Output:** [[1,1]]

**Constraints:**

-   `1 <= candidates.length <= 30`
-   `1 <= candidates[i] <= 200`
-   All elements of  `candidates`  are  **distinct**.
-   `1 <= target <= 500`

# Solution: DFS
```
class Solution {
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        if(candidates.length == 0 || target == 0) return res;
        List<Integer> list = new ArrayList<Integer>();
        dfs(list, 0, 0, candidates, target);
        return res;
    }
    
    public void dfs(List<Integer> list, int index, int sum, int[] candidates, int target){
        if(index >= candidates.length) return;
        dfs(new ArrayList<Integer>(list), index+1, sum, candidates, target);
        
        if(sum + candidates[index] == target){
            list.add(candidates[index]);
            res.add(new ArrayList<Integer>(list));
            return;
        }
        
        if(sum + candidates[index] < target){
            list.add(candidates[index]);
            sum += candidates[index];
            dfs(new ArrayList<Integer>(list),index, sum, candidates, target);
        } 
        
    }
}
```