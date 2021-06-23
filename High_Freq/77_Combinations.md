# 77. Combinations
Given two integers  _n_  and  _k_, return all possible combinations of  _k_  numbers out of 1 ...  _n_.

You may return the answer in  **any order**.

**Example 1:**

**Input:** n = 4, k = 2
**Output:**
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]

**Example 2:**

**Input:** n = 1, k = 1
**Output:** [[1]]

**Constraints:**

-   `1 <= n <= 20`
-   `1 <= k <= n`

# Solution 1: BackTracking (Beat 72%)
```
class Solution {
    
    ArrayList<List<Integer>> res = new ArrayList<List<Integer>>();
    public List<List<Integer>> combine(int n, int k) {
        ArrayList<Integer> cur = new ArrayList<Integer>();
        
        dfs(n,k,cur,0);
        
        return res;
    }
    
    public void dfs(int n, int k, ArrayList<Integer> cur, int index){
        if(index > 0) cur.add(index);
        if(cur.size() == k) res.add(new ArrayList<Integer>(cur));
        
        for(int i = index + 1; i <= n; i++)
            dfs(n,k,cur,i);
        
        if(index > 0) cur.remove(cur.size() - 1);
    }
}
```