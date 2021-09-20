# 57. Insert Interval
Given a set of  _non-overlapping_  intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.

**Example 1:**

**Input:** intervals = [[1,3],[6,9]], newInterval = [2,5]
**Output:** [[1,5],[6,9]]

**Example 2:**

**Input:** intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
**Output:** [[1,2],[3,10],[12,16]]
**Explanation:** Because the new interval `[4,8]` overlaps with `[3,5],[6,7],[8,10]`.

**Example 3:**

**Input:** intervals = [], newInterval = [5,7]
**Output:** [[5,7]]

**Example 4:**

**Input:** intervals = [[1,5]], newInterval = [2,3]
**Output:** [[1,5]]

**Example 5:**

**Input:** intervals = [[1,5]], newInterval = [2,7]
**Output:** [[1,7]]

**Constraints:**

-   `0 <= intervals.length <= 104`
-   `intervals[i].length == 2`
-   `0 <= intervals[i][0] <= intervals[i][1] <= 105`
-   `intervals` is sorted by  `intervals[i][0]`  in  **ascending** order.
-   `newInterval.length == 2`
-   `0 <= newInterval[0] <= newInterval[1] <= 105`

# Solution 1: O(n)
```
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        if(intervals.length == 0) {
            int[][] res = new int[1][2];
            res[0] = newInterval;
            return res;
        }
        List<int[]> arr = new ArrayList<int[]>();
        int index = 0;
        
        while(index < intervals.length && intervals[index][1] < newInterval[0]) arr.add(intervals[index++]);
        
        while(index < intervals.length && intervals[index][0] <= newInterval[1]){
            newInterval[0] = Math.min(newInterval[0],intervals[index][0]);
            newInterval[1] = Math.max(newInterval[1],intervals[index][1]);
            index++;
        }
        
        arr.add(newInterval);
        
        while(index < intervals.length) arr.add(intervals[index++]);
        
        int[][] res= new int[arr.size()][2];
        index = 0;
        for(int[] meet: arr){
            res[index++] = meet;
        }
        return res;
    }
}
```

Better coding:
```
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        List<int[]> arr = new ArrayList<int[]>();
        
        for(int[] cur: intervals){
            if(newInterval == null || cur[1] < newInterval[0]) arr.add(cur);
            else if(cur[0] > newInterval[1]){
                arr.add(newInterval);
                newInterval = null;
                arr.add(cur);
            }else{
                newInterval[0] = Math.min(cur[0], newInterval[0]);
                newInterval[1] = Math.max(cur[1], newInterval[1]);
            }
        }
        if(newInterval != null) arr.add(newInterval);
        
        int index = 0;
        int[][] res = new int[arr.size()][2];
        for(int[] nums:arr){
            res[index++] = nums;
        }
        
        return res;
    }
}
```