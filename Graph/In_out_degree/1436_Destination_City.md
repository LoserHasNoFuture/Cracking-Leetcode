# 1436. Destination City
You are given the array  `paths`, where  `paths[i] = [cityAi, cityBi]`  means there exists a direct path going from  `cityAi`  to  `cityBi`.  _Return the destination city, that is, the city without any path outgoing to another city._

It is guaranteed that the graph of paths forms a line without any loop, therefore, there will be exactly one destination city.

**Example 1:**

**Input:** paths = [["London","New York"],["New York","Lima"],["Lima","Sao Paulo"]]
**Output:** "Sao Paulo" 
**Explanation:** Starting at "London" city you will reach "Sao Paulo" city which is the destination city. Your trip consist of: "London" -> "New York" -> "Lima" -> "Sao Paulo".

**Example 2:**

**Input:** paths = [["B","C"],["D","B"],["C","A"]]
**Output:** "A"
**Explanation:** All possible trips are: 
"D" -> "B" -> "C" -> "A". 
"B" -> "C" -> "A". 
"C" -> "A". 
"A". 
Clearly the destination city is "A".

**Example 3:**

**Input:** paths = [["A","Z"]]
**Output:** "Z"

**Constraints:**

-   `1 <= paths.length <= 100`
-   `paths[i].length == 2`
-   `1 <= cityAi.length, cityBi.length <= 10`
-   `cityAi != cityBi`
-   All strings consist of lowercase and uppercase English letters and the space character.

# Solution 1: My trival HashMap Solution
```
class Solution {
    public String destCity(List<List<String>> paths) {
        HashMap<String, Integer> in_degree = new HashMap<String, Integer>();
        
        for(List<String> path: paths) {
            int cnt = in_degree.getOrDefault(path.get(0),0) + 1;
            in_degree.put(path.get(0),cnt);
            if(!in_degree.containsKey(path.get(1))) in_degree.put(path.get(1),0);
        }
        
        
		for(String key : in_degree.keySet()) {
			if(in_degree.get(key) == 0) return key;
		}
        
        return null;
    }
}
```

# Solution 2: Using HashSet
Refer From: [https://leetcode.com/problems/destination-city/discuss/795620/Java-faster-than-100](https://leetcode.com/problems/destination-city/discuss/795620/Java-faster-than-100)
```
class Solution {
    public String destCity(List<List<String>> paths) {
        HashSet<String> from = new HashSet<String>();
        
        for(List<String> pair : paths){
            from.add(pair.get(0));
        }
        
        for(List<String> pair : paths){
            if(!from.contains(pair.get(1)))
                return pair.get(1);
        }
        
        return null;
    }
}
```