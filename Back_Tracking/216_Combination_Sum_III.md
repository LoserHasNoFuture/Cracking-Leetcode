# 216. Combination Sum III
Find all valid combinations of  `k`  numbers that sum up to  `n`  such that the following conditions are true:

-   Only numbers  `1`  through  `9`  are used.
-   Each number is used  **at most once**.

Return  _a list of all possible valid combinations_. The list must not contain the same combination twice, and the combinations may be returned in any order.

**Example 1:**

**Input:** k = 3, n = 7
**Output:** [[1,2,4]]
**Explanation:**
1 + 2 + 4 = 7
There are no other valid combinations.

**Example 2:**

**Input:** k = 3, n = 9
**Output:** [[1,2,6],[1,3,5],[2,3,4]]
**Explanation:**
1 + 2 + 6 = 9
1 + 3 + 5 = 9
2 + 3 + 4 = 9
There are no other valid combinations.

**Example 3:**

**Input:** k = 4, n = 1
**Output:** []
**Explanation:** There are no valid combinations. [1,2,1] is not valid because 1 is used twice.

**Example 4:**

**Input:** k = 3, n = 2
**Output:** []
**Explanation:** There are no valid combinations.

**Example 5:**

**Input:** k = 9, n = 45
**Output:** [[1,2,3,4,5,6,7,8,9]]
**Explanation:**
1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 = 45
​​​​​​​There are no other valid combinations.

**Constraints:**

-   `2 <= k <= 9`
-   `1 <= n <= 60`

# Solution 1: Back Tracking (100%)
```
class Solution {
    ArrayList<List<Integer>> res = new ArrayList<List<Integer>>(); 
    public List<List<Integer>> combinationSum3(int k, int n) {
        ArrayList<Integer> cur = new ArrayList<Integer>();
        if(k == 1 && n <= 9){
            cur.add(n);
            res.add(cur);
            return res;
        }
        
        if(k >= n || n > 45) return res;
        dfs(cur, k, n, 0);
        return res;
    }
    
    public void dfs(ArrayList<Integer> cur, int k, int n, int index){
        if(n < 0 || k < 0) return;
        if(n == 0 && k == 0) res.add(new ArrayList<Integer>(cur));
        
        for(int i = index + 1; i <= 9; i++){
            if(n - i < 0) break;
            cur.add(i);
            dfs(cur,k-1,n-i,i);
            cur.remove(cur.size()-1);
        }
    }
}
```