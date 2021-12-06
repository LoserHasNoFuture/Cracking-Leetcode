# 332. Reconstruct Itinerary 2097
You are given a list of airline  `tickets`  where  `tickets[i] = [fromi, toi]`  represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from  `"JFK"`, thus, the itinerary must begin with  `"JFK"`. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

-   For example, the itinerary  `["JFK", "LGA"]`  has a smaller lexical order than  `["JFK", "LGB"]`.

You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/itinerary1-graph.jpg)

**Input:** tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]
**Output:** ["JFK","MUC","LHR","SFO","SJC"]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/itinerary2-graph.jpg)

**Input:** tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
**Output:** ["JFK","ATL","JFK","SFO","ATL","SFO"]
**Explanation:** Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"] but it is larger in lexical order.

**Constraints:**

-   `1 <= tickets.length <= 300`
-   `tickets[i].length == 2`
-   `fromi.length == 3`
-   `toi.length == 3`
-   `fromi`  and  `toi`  consist of uppercase English letters.
-   `fromi  != toi`

# Solution: Eulerian Path
```
class Solution {
    HashMap<String, List<String>> graph = new HashMap<String, List<String>>();
    HashMap<String, Integer> degree = new HashMap<String, Integer>();
    
    public List<String> findItinerary(List<List<String>> tickets) {
        build_graph(tickets);
        
        List<String> arr = new ArrayList<String>();
        
        
        sort_links();
        dfs("JFK", arr);
        
        int n = arr.size();
        List<String> res = new ArrayList<String>();
        for(int i = n-1; i >=0; i--) res.add(arr.get(i));
        
        return res;
    }
    
    public void sort_links(){
        for(String key: graph.keySet()){
            List<String> arr = graph.get(key);
            Collections.sort(arr,(a,b)->b.compareTo(a));
        }
    }
    
    public void dfs(String cur, List<String> arr){
        List<String> stack = graph.get(cur);
        if(stack == null || stack.size() == 0) {
            // very important!!! Or else it may add an extra Node which is the start of the loop
            // example: [["JFK","KUL"],["JFK","NRT"],["NRT","JFK"]]
            if(arr.size() > 0 && arr.get(arr.size()-1).equals(cur)) return;
            arr.add(cur);
            return;
        }
        
        while(stack.size() > 0){
            String next = stack.get(stack.size()-1);
            stack.remove(stack.size()-1);
            dfs(next, arr);
            
            arr.add(cur);
        }
        
    }
    
    public String find_start_point(){
        String start = "";
        
        for(String point: degree.keySet()){
            if(degree.get(point) < 0){
                start = point;
                break;
            }
        }
        
        if(start.length() == 0){
            for(String point: degree.keySet()){
                start = point;
                break;
            }
        }
        
        return start;
    }
    
    public void build_graph(List<List<String>> pairs){
        for(List<String> pair: pairs){
            List<String> stack;
            if(graph.containsKey(pair.get(0))){
                stack = graph.get(pair.get(0));
                stack.add(pair.get(1));
            }else{
                stack = new ArrayList<String>();
                stack.add(pair.get(1));
                graph.put(pair.get(0), stack);
            }
            degree.put(pair.get(1), degree.getOrDefault(pair.get(1), 0)+1);
            degree.put(pair.get(0), degree.getOrDefault(pair.get(0), 0)-1);
        }
    }
}
``` 