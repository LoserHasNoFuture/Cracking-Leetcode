# 254. Factor Combinations
Numbers can be regarded as product of its factors. For example,

8 = 2 x 2 x 2;
  = 2 x 4.

Write a function that takes an integer  _n_  and return all possible combinations of its factors.

**Note:**

1.  You may assume that  _n_  is always positive.
2.  Factors should be greater than 1 and less than  _n_.

**Example 1:**

**Input:** `1`
**Output:** []

**Example 2:**

**Input:** `37`
**Output:**[]

**Example 3:**

**Input:** `12`
**Output:**
[
  [2, 6],
  [2, 2, 3],
  [3, 4]
]

**Example 4:**

**Input:** `32`
**Output:**
[
  [2, 16],
  [2, 2, 8],
  [2, 2, 2, 4],
  [2, 2, 2, 2, 2],
  [2, 4, 4],
  [4, 8]
]


# Solution 1: BackTracking (Beat 67%)
```
class Solution {
    ArrayList<List<Integer>> res = new ArrayList<List<Integer>>();
    public List<List<Integer>> getFactors(int n) {
        if(n <= 1) return res;
        ArrayList<Integer> cur = new ArrayList<Integer>();
        dfs(n, 2,cur);
        
        return res;
    }
    
    public void dfs(int n, int fac, ArrayList<Integer> cur){
        
        int min = n;
        for(int i = fac; i < min; i++){
            if(n%i == 0) {
                min = n/i;
                if(min < i) break;
                
                cur.add(i); cur.add(n/i);
                res.add(new ArrayList<Integer>(cur));
                cur.remove(cur.size()-1);
                
                dfs(min, i , cur);
                cur.remove(cur.size()-1);
            }
        }
        
    }
}
```