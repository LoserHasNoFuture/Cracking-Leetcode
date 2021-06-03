# 739. Daily Temperatures
Given an array of integers  `temperatures`  represents the daily temperatures, return  _an array_  `answer`  _such that_  `answer[i]`  _is the number of days you have to wait after the_  `ith`  _day to get a warmer temperature_. If there is no future day for which this is possible, keep  `answer[i] == 0`  instead.

**Example 1:**

**Input:** temperatures = [73,74,75,71,69,72,76,73]
**Output:** [1,1,4,2,1,1,0,0]

**Example 2:**

**Input:** temperatures = [30,40,50,60]
**Output:** [1,1,1,0]

**Example 3:**

**Input:** temperatures = [30,60,90]
**Output:** [1,1,0]

**Constraints:**

-   `1 <= temperatures.length <= 105`
-   `30 <= temperatures[i] <= 100`

# Solution 1: Using Stack (Beat 65%)
```
class Solution {
    
    public int[] dailyTemperatures(int[] temp) {
        Deque<int[]> res = new ArrayDeque<int[]>();
        int[] arr = new int[temp.length];
        
        for(int i = temp.length-1; i >= 0; i--){
            int cnt = 0;
            while(!res.isEmpty() && res.peek()[0] <= temp[i]) {
                cnt += res.pop()[1];
            }
            
            if(!res.isEmpty()) cnt++;
            else cnt = 0;
            res.push(new int[]{temp[i],cnt});
            arr[i] = cnt;
            
        }
        
        return arr;
    }
}
```

# Solution 2: Using array to realize stack (Beat 100%)
```
class Solution {
    public int[] dailyTemperatures(int[] temp) {
        int[] res = new int[temp.length];
        int[] index = new int[temp.length];
        int last = temp.length;
        for(int i = temp.length - 1; i >= 0; i--){
            while(last < temp.length && temp[index[last]] <= temp[i]) res[i] += res[index[last++]];
            if(last == temp.length) res[i] = 0;
            else res[i]++;
            index[--last] = i;
        }
        
        return res;
    }
}
```