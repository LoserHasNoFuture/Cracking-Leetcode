# 621. Task Scheduler
Given a characters array  `tasks`, representing the tasks a CPU needs to do, where each letter represents a different task. Tasks could be done in any order. Each task is done in one unit of time. For each unit of time, the CPU could complete either one task or just be idle.

However, there is a non-negative integer `n`  that represents the cooldown period between two  **same tasks** (the same letter in the array), that is that there must be at least  `n`  units of time between any two same tasks.

Return  _the least number of units of times that the CPU will take to finish all the given tasks_.

**Example 1:**

**Input:** tasks = ["A","A","A","B","B","B"], n = 2
**Output:** 8
**Explanation:** 
A -> B -> idle -> A -> B -> idle -> A -> B
There is at least 2 units of time between any two same tasks.

**Example 2:**

**Input:** tasks = ["A","A","A","B","B","B"], n = 0
**Output:** 6
**Explanation:** On this case any permutation of size 6 would work since n = 0.
["A","A","A","B","B","B"]
["A","B","A","B","A","B"]
["B","B","B","A","A","A"]
...
And so on.

**Example 3:**

**Input:** tasks = ["A","A","A","A","A","A","B","C","D","E","F","G"], n = 2
**Output:** 16
**Explanation:** 
One possible solution is
A -> B -> C -> A -> D -> E -> A -> F -> G -> A -> idle -> idle -> A -> idle -> idle -> A

**Constraints:**

-   `1 <= task.length <= 104`
-   `tasks[i]`  is upper-case English letter.
-   The integer  `n`  is in the range  `[0, 100]`.

# Solution 1: Sort and Schedule (Beat 40%)
```
class Solution {
    public int leastInterval(char[] tasks, int n) {
        if(n == 0) return tasks.length;
        int[] count = new int[26];
        for(char task: tasks) count[task-'A']++;
        Arrays.sort(count);
        reverse(count);
        
        int res = 0;
        while(count[0] != 0){
            int i = 0;
            for(; i < 26 && count[i] > 0 && i <= n; i++) count[i]--;
            
            if(count[0] != 0) res += Math.max(i,n + 1);
            else res += Math.min(i,n + 1);
            
            Arrays.sort(count);
            reverse(count);
        }
        
        return res;
    }
    
    public void reverse(int[] count){
        int len = count.length;
        for(int i = 0; i < len/2; i++){
            swap(count,i,len-1-i);
        }
    }
    
    public void swap(int[] count, int a, int b){
        int tmp = count[a];
        count[a] = count[b];
        count[b] = tmp;
    }
}
```

# Solution 2: O(n) solution (Beat 99%)
Refer from: [https://leetcode.com/problems/task-scheduler/discuss/104500/Java-O(n)-time-O(1)-space-1-pass-no-sorting-solution-with-detailed-explanation](https://leetcode.com/problems/task-scheduler/discuss/104500/Java-O(n)-time-O(1)-space-1-pass-no-sorting-solution-with-detailed-explanation)
```
public class Solution {
    public int leastInterval(char[] tasks, int n) {
        int[] counter = new int[26];
        int max = 0;
        int maxCount = 0;
        for(char task : tasks) {
            counter[task - 'A']++;
            if(max == counter[task - 'A']) {
                maxCount++;
            }
            else if(max < counter[task - 'A']) {
                max = counter[task - 'A'];
                maxCount = 1;
            }
        }
        
        int partCount = max - 1;
        int partLength = n - (maxCount - 1);
        int emptySlots = partCount * partLength;
        int availableTasks = tasks.length - max * maxCount;
        int idles = Math.max(0, emptySlots - availableTasks);
        
        return tasks.length + idles;
    }
}
```


Revision based on his method:
```
public class Solution {
    public int leastInterval(char[] tasks, int n) {
        if(n == 0) return tasks.length;
        int max = 0;
        int maxCount = 0;
        int[] count = new int[26];
        for(char c: tasks){
            count[c-'A']++;
            if(count[c-'A'] == max) maxCount++;
            else if(max < count[c-'A']){
                max = count[c-'A'];
                maxCount = 1;
            }
        }
        
        return Math.max(tasks.length, (max-1)*(n+1) + maxCount);
    }
}
```