# 364. Nested List Weight Sum II
Given a nested list of integers, return the sum of all integers in the list weighted by their depth.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

Different from the  [previous question](https://leetcode.com/problems/nested-list-weight-sum/)  where weight is increasing from root to leaf, now the weight is defined from bottom up. i.e., the leaf level integers have weight 1, and the root level integers have the largest weight.

**Example 1:**

**Input:** [[1,1],2,[1,1]]
**Output:** 8 
**Explanation:** Four 1's at depth 1, one 2 at depth 2.

**Example 2:**

**Input:** [1,[4,[6]]]
**Output:** 17 
**Explanation:** One 1 at depth 3, one 4 at depth 2, and one 6 at depth 1; 1*3 + 4*2 + 6*1 = 17.


# Solution 1: DFS (Beat 50%)
```
class Solution {
    int max = 1;
    int depth = 1;
    HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
    public int depthSumInverse(List<NestedInteger> nestedList) {
        if(nestedList == null) return 0;
        dfs(nestedList);
        
        int res  = 0;
		for(Integer key: map.keySet()) {
			int sum = map.get(key);
            res += (max-key + 1) * sum;
		}
        return res;
    }
    
    public void dfs(List<NestedInteger> nestedList){
        if(nestedList == null) return;
        max = Math.max(depth, max);
        for(NestedInteger list : nestedList){
            if(list.isInteger()){
                int sum = map.getOrDefault(depth,0);
                sum += list.getInteger();
                map.put(depth,sum);
            }else{
                depth++;
                dfs(list.getList());
                depth--;
            }
        }
    }
}
```

# Solution 2: DFS + Math (Beat 100%)
Refer from: [https://leetcode.com/problems/nested-list-weight-sum-ii/discuss/114195/Java-one-pass-DFS-solution-mathematically](https://leetcode.com/problems/nested-list-weight-sum-ii/discuss/114195/Java-one-pass-DFS-solution-mathematically)

Key idea: 1x + 2y + 3z = (3 + 1) * (x + y + z) - (3x + 2y + z)
```
class Solution {
    int sum = 0, depth = 1, max = 1, res = 0;
    public int depthSumInverse(List<NestedInteger> nestedList) {
        dfs(nestedList);
        res = (res * (max + 1)) - sum; 
        return res;
    }
    
    public void dfs(List<NestedInteger> nestedList){
        if(nestedList == null) return;
        max = Math.max(depth,max);
        for(NestedInteger ni : nestedList){
            if(ni.isInteger()) {
                sum += (depth*ni.getInteger());
                res += ni.getInteger();
            }else{
                depth++;
                dfs(ni.getList());
                depth--;
            }
        }
    }
}
```

# Solution 3: BFS (Beat 100%)
Refer from: [https://leetcode.com/problems/nested-list-weight-sum-ii/discuss/83641/No-depth-variable-no-multiplication](https://leetcode.com/problems/nested-list-weight-sum-ii/discuss/83641/No-depth-variable-no-multiplication)

```
class Solution {
    
    public int depthSumInverse(List<NestedInteger> nestedList) {
    // Key is level_result, it only is initiated once!
        int res = 0, level_result = 0;
        
        while(!nestedList.isEmpty()){
            List<NestedInteger> next_level = new ArrayList<NestedInteger>();
            for(NestedInteger ni : nestedList){
                if(ni.isInteger()) level_result += ni.getInteger();
                else next_level.addAll(ni.getList());
            }
            res += level_result;
            nestedList = next_level;
        }
        
        return res;
    }
    
}
```