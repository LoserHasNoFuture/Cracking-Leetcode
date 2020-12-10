# 40. Combination Sum II
Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in  `candidates` where the candidate numbers sum to  `target`.

Each number in  `candidates` may only be used  **once**  in the combination.

**Note:** The solution set must not contain duplicate combinations.

**Example 1:**

**Input:** candidates = [10,1,2,7,6,1,5], target = 8
**Output:** 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]

**Example 2:**

**Input:** candidates = [2,5,2,1,2], target = 5
**Output:** 
[
[1,2,2],
[5]
]

**Constraints:**

-   `1 <= candidates.length <= 100`
-   `1 <= candidates[i] <= 50`
-   `1 <= target <= 30`

# Solution 1: BackTracking (Beat 99%)
```
class Solution {
    ArrayList<List<Integer>> res = new ArrayList<List<Integer>>();
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        int n = candidates.length;
        
        Arrays.sort(candidates);
        ArrayList<Integer> cur = new ArrayList<Integer>();
        
        HashSet<Integer> set = new HashSet<Integer>();
        for(int i = 0; i < n; i++){
            if(!set.contains(candidates[i])){
                set.add(candidates[i]);
                dfs(candidates,target,0,i,cur);
            }
        }
        
        return res;
    }
    
    public void dfs(int[] candidates, int target, int sum, int index, ArrayList<Integer> cur){
        if(sum >= target) return;
        
        cur.add(candidates[index]);
        sum += candidates[index];
        
        if(sum == target) res.add(new ArrayList<Integer>(cur));
        
        
        for(int i = index+1; i < candidates.length; i++){
            if(sum + candidates[i] > target) break;
            if(i > index+1 && candidates[i] == candidates[i-1]) continue;
            dfs(candidates,target,sum,i,cur);
        
        }
        
        sum -= candidates[index];
        cur.remove(cur.size()-1);
        
    }
}
```