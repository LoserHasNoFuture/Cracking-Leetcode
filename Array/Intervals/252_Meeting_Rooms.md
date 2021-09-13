# 252. Meeting Rooms
Given an array of meeting time  `intervals` where  `intervals[i] = [starti, endi]`, determine if a person could attend all meetings.

**Example 1:**

**Input:** intervals = [[0,30],[5,10],[15,20]]
**Output:** false

**Example 2:**

**Input:** intervals = [[7,10],[2,4]]
**Output:** true

**Constraints:**

-   `0 <= intervals.length <= 104`
-   `intervals[i].length == 2`
-   `0 <= starti  < endi  <= 106`

# Solution 1: Sort Start
```
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        if(intervals.length == 0) return true;
        Arrays.sort(intervals, (a,b)->(a[0]-b[0]));
        
        for(int i = 1; i < intervals.length; i++){
            if(intervals[i][0] < intervals[i-1][1]) return false;
        }
        
        return true;
    }
}
```

# Solution 2: Sort Start & End Seperately (Beat 100%)
```
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        int n = intervals.length;
        int[] start = new int[n];
        int[] end = new int[n];
        
        for(int i = 0; i < n; i++){
            start[i] = intervals[i][0];
            end[i] = intervals[i][1];
        }
        
        Arrays.sort(start);
        Arrays.sort(end);
        
        for(int i = 0; i < n - 1; i++){
            if(end[i] > start[i + 1]) return false;
        }
        
        return true;
    }
}
```

# Solution 3: Sweeping Line O(nlogn)
```
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        if(intervals.length <= 1) return true;
        int n = intervals.length, index = 0, cnt = 0;
        int[][] nums = new int[n*2][2];
        for(int[] cur: intervals){
            nums[index++] = new int[]{cur[0],1};
            nums[index++] = new int[]{cur[1],-1};
        }
        Arrays.sort(nums, (a,b)->(a[0]==b[0]?a[1]-b[1]:a[0]-b[0]));
        
        for(int[] num: nums){
            cnt += num[1];
            if(cnt > 1) return false;
        }
        
        return true;
    }
}
```
