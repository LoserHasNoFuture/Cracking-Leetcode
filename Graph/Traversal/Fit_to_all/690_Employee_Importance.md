# 690. Employee Importance
You are given a data structure of employee information, which includes the employee's  **unique id**, their **importance value**  and their **direct**  subordinates' id.

For example, employee 1 is the leader of employee 2, and employee 2 is the leader of employee 3. They have importance value 15, 10 and 5, respectively. Then employee 1 has a data structure like [1, 15, [2]], and employee 2 has [2, 10, [3]], and employee 3 has [3, 5, []]. Note that although employee 3 is also a subordinate of employee 1, the relationship is  **not direct**.

Now given the employee information of a company, and an employee id, you need to return the total importance value of this employee and all their subordinates.

**Example 1:**

**Input:** [[1, 5, [2, 3]], [2, 3, []], [3, 3, []]], 1
**Output:** 11
**Explanation:**
Employee 1 has importance value 5, and he has two direct subordinates: employee 2 and employee 3. They both have importance value 3. So the total importance value of employee 1 is 5 + 3 + 3 = 11.

**Note:**

1.  One employee has at most one  **direct**  leader and may have several subordinates.
2.  The maximum number of employees won't exceed 2000.

# Solution 1: BFS
```
class Solution {
    public int getImportance(List<Employee> employees, int id) {
        HashMap<Integer, Employee> map = new HashMap<Integer, Employee>();
        for(int i = 0; i < employees.size(); i++) map.put(employees.get(i).id, employees.get(i));
        
        Queue<Integer> queue = new LinkedList<Integer>();
        HashSet<Integer> visited = new HashSet<Integer>();
        queue.offer(id);
        visited.add(id);
        
        int res = 0;
        while(!queue.isEmpty()){
            Employee emp = map.get(queue.poll());
            res += emp.importance;
            
            for(int i = 0; i < emp.subordinates.size(); i++){
                int sub = emp.subordinates.get(i);
                if(!visited.contains(sub)) {
                    visited.add(sub);
                    queue.offer(sub);
                }
            }
        }
        
        return res;
    }
}
```

# Solution: DFS
```
class Solution {
    HashMap<Integer, Employee> map = new HashMap<Integer, Employee>();
    HashSet<Integer> visited = new HashSet<Integer>();
    int res = 0;
    public int getImportance(List<Employee> employees, int id) {
        for(int i = 0; i < employees.size(); i++) map.put(employees.get(i).id, employees.get(i));
        dfs(id);
        return res;
    }
    
    public void dfs(int id){
        visited.add(id);
        
        Employee emp = map.get(id);
        res += emp.importance;
        
        for(int i = 0; i < emp.subordinates.size(); i++){
            int sub = emp.subordinates.get(i);
            if(!visited.contains(sub)) dfs(sub);
        }
    }
}
```