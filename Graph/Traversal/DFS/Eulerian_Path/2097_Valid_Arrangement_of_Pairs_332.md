# 2097. Valid Arrangement of Pairs 332
You are given a  **0-indexed**  2D integer array  `pairs`  where  `pairs[i] = [starti, endi]`. An arrangement of  `pairs`  is  **valid**  if for every index  `i`  where  `1 <= i < pairs.length`, we have  `endi-1  == starti`.

Return  _**any**  valid arrangement of_ `pairs`.

**Note:**  The inputs will be generated such that there exists a valid arrangement of  `pairs`.

**Example 1:**

**Input:** pairs = [[5,1],[4,5],[11,9],[9,4]]
**Output:** [[11,9],[9,4],[4,5],[5,1]]
**Explanation:** 
```
This is a valid arrangement since endi-1 always equals starti.
end0 = 9 == 9 = start1 
end1 = 4 == 4 = start2
end2 = 5 == 5 = start3
```

**Example 2:**

**Input:** pairs = [[1,3],[3,2],[2,1]]
**Output:** [[1,3],[3,2],[2,1]]
**Explanation:**
```
This is a valid arrangement since endi-1 always equals starti.
end0 = 3 == 3 = start1
end1 = 2 == 2 = start2
The arrangements [[2,1],[1,3],[3,2]] and [[3,2],[2,1],[1,3]] are also valid.
```

**Example 3:**

**Input:** pairs = [[1,2],[1,3],[2,1]]
**Output:** [[1,2],[2,1],[1,3]]
**Explanation:**
```
This is a valid arrangement since endi-1 always equals starti.
end0 = 2 == 2 = start1
end1 = 1 == 1 = start2
```

**Constraints:**

-   `1 <= pairs.length <= 105`
-   `pairs[i].length == 2`
-   `0 <= starti, endi  <= 109`
-   `starti  != endi`
-   No two pairs are exactly the same.
-   There  **exists**  a valid arrangement of  `pairs`.

# Solution: Eulerian Path
Refer from: https://leetcode.com/problems/valid-arrangement-of-pairs/discuss/1611978/C%2B%2B-Eulerian-Path-with-Explanation

```
class Solution {
    HashMap<Integer, List<Integer>> graph = new HashMap<Integer, List<Integer>>();
    HashMap<Integer, Integer> degree = new HashMap<Integer, Integer>();
    public int[][] validArrangement(int[][] pairs) {
        build_graph(pairs);
        
        int n = pairs.length, index = n;
        
        int start = find_start_point();
        List<int[]> arr = new ArrayList<int[]>();
        dfs(start, arr);
        
        int[][] res = new int[n][2];
        for(int[] point: arr){
            res[--index] = point;
        }
        
        return res;
    }
    
    public void dfs(int cur, List<int[]> arr){
        List<Integer> stack = graph.get(cur);
        if(stack == null) return;
        while(stack.size() != 0){
            int next = stack.get(stack.size()-1);
            stack.remove(stack.size()-1);
            dfs(next,arr);
            arr.add(new int[]{cur, next});
        }
        
        
    }
    
    public int find_start_point(){
        int start = -1;
        
        for(int point: degree.keySet()){
            start = point;
            if(degree.get(point) < 0){
                break;
            }
        }
        
        return start;
    }
    
    
    public void build_graph(int[][] pairs){
        for(int[] pair: pairs){
            List<Integer> stack;
            if(graph.containsKey(pair[0])){
                stack = graph.get(pair[0]);
                stack.add(pair[1]);
            }else{
                stack = new ArrayList<Integer>();
                stack.add(pair[1]);
                graph.put(pair[0], stack);
            }
            degree.put(pair[1], degree.getOrDefault(pair[1], 0)+1);
            degree.put(pair[0], degree.getOrDefault(pair[0], 0)-1);
        }
    }
}
```