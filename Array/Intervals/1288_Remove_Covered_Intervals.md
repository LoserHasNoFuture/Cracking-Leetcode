# 1288. Remove Covered Intervals
Given a list of  `intervals`, remove all intervals that are covered by another interval in the list.

Interval  `[a,b)`  is covered by interval  `[c,d)`  if and only if  `c <= a`  and  `b <= d`.

After doing so, return  _the number of remaining intervals_.

**Example 1:**

**Input:** intervals = [[1,4],[3,6],[2,8]]
**Output:** 2
**Explanation:** Interval [3,6] is covered by [2,8], therefore it is removed.

**Example 2:**

**Input:** intervals = [[1,4],[2,3]]
**Output:** 1

**Example 3:**

**Input:** intervals = [[0,10],[5,12]]
**Output:** 2

**Example 4:**

**Input:** intervals = [[3,10],[4,10],[5,11]]
**Output:** 2

**Example 5:**

**Input:** intervals = [[1,2],[1,4],[3,4]]
**Output:** 1

**Constraints:**

-   `1 <= intervals.length <= 1000`
-   `intervals[i].length == 2`
-   `0 <= intervals[i][0] < intervals[i][1] <= 10^5`
-   All the intervals are  **unique**.

# Solution 1: Sort Start Time (Beat 96%)
```
class Solution {
    public int removeCoveredIntervals(int[][] intervals) {
        int n = intervals.length;
        if(n <= 1) return n;
        Arrays.sort(intervals, (a,b)->(a[0]==b[0]? b[1]-a[1]:a[0]-b[0]));
        
        int cnt = 0;
        int start = intervals[0][0], end = intervals[0][1];
        for(int i = 1; i < n; i++){
            if(intervals[i][0] >= start && intervals[i][1] <= end) cnt++;
            else if(intervals[i][1] > end){
                start = intervals[i][0];
                end = intervals[i][1]; 
            }
            
        }
        
        return n - cnt;
    }
}
```
