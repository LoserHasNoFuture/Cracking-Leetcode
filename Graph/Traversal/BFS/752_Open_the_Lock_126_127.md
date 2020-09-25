# 752. Open the Lock
You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots:  `'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'`. The wheels can rotate freely and wrap around: for example we can turn  `'9'`  to be  `'0'`, or  `'0'`  to be  `'9'`. Each move consists of turning one wheel one slot.

The lock initially starts at  `'0000'`, a string representing the state of the 4 wheels.

You are given a list of  `deadends`  dead ends, meaning if the lock displays any of these codes, the wheels of the lock will stop turning and you will be unable to open it.

Given a  `target`  representing the value of the wheels that will unlock the lock, return the minimum total number of turns required to open the lock, or -1 if it is impossible.

**Example 1:**

**Input:** deadends = ["0201","0101","0102","1212","2002"], target = "0202"
**Output:** 6
**Explanation:**
A sequence of valid moves would be "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202".
Note that a sequence like "0000" -> "0001" -> "0002" -> "0102" -> "0202" would be invalid,
because the wheels of the lock become stuck after the display becomes the dead end "0102".

**Example 2:**

**Input:** deadends = ["8888"], target = "0009"
**Output:** 1
**Explanation:**
We can turn the last wheel in reverse to move from "0000" -> "0009".

**Example 3:**

**Input:** deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"], target = "8888"
**Output:** -1
Explanation:
We can't reach the target without getting stuck.

**Example 4:**

**Input:** deadends = ["0000"], target = "8888"
**Output:** -1

**Constraints:**

-   `1 <= deadends.length <= 500`
-   `deadends[i].length == 4`
-   `target.length == 4`
-   target  **will not be**  in the list  `deadends`.
-   `target`  and  `deadends[i]`  consist of digits only.

# Solution: One-End BFS
```
class Solution {
    public int openLock(String[] deadends, String target) {
        if(target.equals("0000")) return 0;
        int res = 0;
        
        HashSet<String> visited = new HashSet<String>();
        for(int i = 0; i < deadends.length; i++) visited.add(deadends[i]);
        if(visited.contains("0000")) return -1;
        visited.add("0000");
        
        Queue<String> queue = new LinkedList<String>();
        queue.offer("0000");
        
        while(!queue.isEmpty()){
            int sz = queue.size();
            
            for(int i = 0; i < sz; i++){
                String code = queue.poll();
                if(code.equals(target)) return res;
                for(int j = 0; j < code.length(); j++){
                    String up = turnUp(code,j);
                    if(!visited.contains(up)) {
                        queue.offer(up);
                        visited.add(up);
                    }
                    String down = turnDown(code,j);
                    if(!visited.contains(down)) {
                        queue.offer(down);
                        visited.add(down);
                    }
                }
            }   
            res++;
        }
        return -1;
    }
    
    
    public String turnUp(String code, int index){
        StringBuilder sb = new StringBuilder(code);
        int num = sb.charAt(index) - '0';
        if(num < 9) num++;
        else num = 0;
        sb.replace(index,index+1,num+"");
        return sb.toString();
    }
    
    public String turnDown(String code, int index){
        StringBuilder sb = new StringBuilder(code);
        int num = sb.charAt(index) - '0';
        if(num > 0) num--;
        else num = 9;
        sb.replace(index,index+1,num+"");
        return sb.toString();
    }
    
}
```

# Solution : Two-end BFS
```
class Solution {
    public int openLock(String[] deadends, String target) {
        if(target.equals("0000")) return 0;
        int res = 0;
        
        HashSet<String> visited1 = new HashSet<String>();
        for(int i = 0; i < deadends.length; i++) visited1.add(deadends[i]);
        if(visited1.contains("0000")) return -1;
        visited1.add("0000");
        
        Queue<String> queue1 = new LinkedList<String>();
        queue1.offer("0000");
        
        
        HashSet<String> visited2 = new HashSet<String>();
        for(int i = 0; i < deadends.length; i++) visited2.add(deadends[i]);
        Queue<String> queue2 = new LinkedList<String>();
        queue2.offer(target);
        
        
        while(!queue1.isEmpty() && !queue2.isEmpty()){
            
            if(queue2.size() < queue1.size()){
                Queue<String> temp = queue1;
                queue1 = queue2;
                queue2 = temp;
                HashSet<String> temp_visited = visited1;
                visited1 = visited2;
                visited2 = temp_visited;
            }
            
            
            int sz = queue1.size();
            
            for(int i = 0; i < sz; i++){
                String code = queue1.poll();
                if(visited2.contains(code)) return res;
                for(int j = 0; j < code.length(); j++){
                    String up = turnUp(code,j);
                    if(!visited1.contains(up)) {
                        queue1.offer(up);
                        visited1.add(up);
                    }
                    String down = turnDown(code,j);
                    if(!visited1.contains(down)) {
                        queue1.offer(down);
                        visited1.add(down);
                    }
                }
            }
            
            res++;
        }
             
        return -1;
    }
    
    
    public String turnUp(String code, int index){
        StringBuilder sb = new StringBuilder(code);
        int num = sb.charAt(index) - '0';
        if(num < 9) num++;
        else num = 0;
        sb.replace(index,index+1,num+"");
        return sb.toString();
    }
    
    public String turnDown(String code, int index){
        StringBuilder sb = new StringBuilder(code);
        int num = sb.charAt(index) - '0';
        if(num > 0) num--;
        else num = 9;
        sb.replace(index,index+1,num+"");
        return sb.toString();
    }
    
}
```
