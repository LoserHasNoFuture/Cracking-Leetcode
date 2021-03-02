# 435. Non-overlapping Intervals
Given a collection of intervals, find the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

**Example 1:**

**Input:** [[1,2],[2,3],[3,4],[1,3]]
**Output:** 1
**Explanation:** [1,3] can be removed and the rest of intervals are non-overlapping.

**Example 2:**

**Input:** [[1,2],[1,2],[1,2]]
**Output:** 2
**Explanation:** You need to remove two [1,2] to make the rest of intervals non-overlapping.

**Example 3:**

**Input:** [[1,2],[2,3]]
**Output:** 0
**Explanation:** You don't need to remove any of the intervals since they're already non-overlapping.

**Note:**

1.  You may assume the interval's end point is always bigger than its start point.
2.  Intervals like [1,2] and [2,3] have borders "touching" but they don't overlap each other.

# Solution 1: Sort Start Time (Beat 100%)
```
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        int n = intervals.length;
        if(n <= 1) return 0;
        Arrays.sort(intervals,(a,b)->(a[0]-b[0]));
        
        int end = intervals[0][1];
        int res = 0;
        for(int i = 1; i < n; i++){
            if(intervals[i][0] < end){
                if(intervals[i][1] < end) end = intervals[i][1];
                res++;
            }else end = Math.max(intervals[i][1], end);
        }
        
        return res;
    }
}
```

# Solution 2: Sort End Time (Beat 60%)
**Seems to me, second solution is easier**
```
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        int n = intervals.length;
        if(n <= 1) return 0;
        Arrays.sort(intervals,(a,b)->(a[1]-b[1]));
        
        int cnt = 0, end = intervals[0][1];
        for(int i = 1; i < n; i++){
            if(intervals[i][0] < end) cnt++;
            else end = intervals[i][1];
        }
        
        return cnt;
    }
}
```

# We can not sort start and end arrays sparately
consider this case: [[-100,-87],[-99,-44],[-85-76],[-76,-10]]