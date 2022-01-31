# 399. Evaluate Division
You are given an array of variable pairs  `equations`  and an array of real numbers  `values`, where  `equations[i] = [Ai, Bi]`  and  `values[i]`  represent the equation  `Ai  / Bi  = values[i]`. Each  `Ai`  or  `Bi`  is a string that represents a single variable.

You are also given some  `queries`, where  `queries[j] = [Cj, Dj]`  represents the  `jth`  query where you must find the answer for  `Cj  / Dj  = ?`.

Return  _the answers to all queries_. If a single answer cannot be determined, return  `-1.0`.

**Note:**  The input is always valid. You may assume that evaluating the queries will not result in division by zero and that there is no contradiction.

**Example 1:**

**Input:** equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
**Output:** [6.00000,0.50000,-1.00000,1.00000,-1.00000]
**Explanation:** 
Given: _a / b = 2.0_, _b / c = 3.0_
queries are: _a / c = ?_, _b / a = ?_, _a / e = ?_, _a / a = ?_, _x / x = ?_
return: [6.0, 0.5, -1.0, 1.0, -1.0 ]

**Example 2:**

**Input:** equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
**Output:** [3.75000,0.40000,5.00000,0.20000]

**Example 3:**

**Input:** equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
**Output:** [0.50000,2.00000,-1.00000,-1.00000]

**Constraints:**

-   `1 <= equations.length <= 20`
-   `equations[i].length == 2`
-   `1 <= Ai.length, Bi.length <= 5`
-   `values.length == equations.length`
-   `0.0 < values[i] <= 20.0`
-   `1 <= queries.length <= 20`
-   `queries[i].length == 2`
-   `1 <= Cj.length, Dj.length <= 5`
-   `Ai, Bi, Cj, Dj`  consist of lower case English letters and digits.

# Solution: DFS
```
class Solution {
    
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
        HashMap<String, HashMap<String, Double>> graph = build_graph(equations, values);
        
        int n = queries.size();
        double[] res = new double[n];
        
        for(int i = 0; i < n; i++){
            String src = queries.get(i).get(0);
            String dst = queries.get(i).get(1);
            
            res[i] = dfs(graph, src, dst, new HashSet<String>());
        }
        
        return res;
    }
    
    public double dfs(HashMap<String, HashMap<String, Double>> graph, 
                      String src, String dst, HashSet<String> visited){
        if(!graph.containsKey(src)) return -1.0;
        visited.add(src);
        HashMap<String, Double> links = graph.get(src);
        if(links.containsKey(dst)) return links.get(dst);
        
        for(String next: links.keySet()){
            if(visited.contains(next)) continue;
            double cur = dfs(graph, next, dst, visited);
            if(cur != -1) return cur*links.get(next);
        }
        
        return -1.0;
    }
    
    public HashMap<String, HashMap<String, Double>> build_graph(List<List<String>> equations, double[] values){
        HashMap<String, HashMap<String, Double>> graph = new HashMap<String, HashMap<String, Double>>();
        
        int n = values.length;
        for(int i = 0; i < n; i++){
            String src = equations.get(i).get(0);
            String dst = equations.get(i).get(1);
            double weight = values[i];
            
            graph.putIfAbsent(src, new HashMap<String, Double>());
            graph.get(src).put(dst, weight);
            graph.putIfAbsent(dst, new HashMap<String, Double>());
            graph.get(dst).put(src, 1/weight);
        }
        
        return graph;
    }
}
```