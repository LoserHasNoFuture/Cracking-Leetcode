# 253. Meeting Rooms II
Given an array of meeting time intervals consisting of start and end times  `[[s1,e1],[s2,e2],...]`  (si  < ei), find the minimum number of conference rooms required.

**Example 1:**

**Input:** `[[0, 30],[5, 10],[15, 20]]`
**Output:** 2

**Example 2:**

**Input:** [[7,10],[2,4]]
**Output:** 1

**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

# Solution 1: Sort Start & Priority Queue (Beat 78%)
```
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        int n = intervals.length;
        if(n <= 1) return n;
        Arrays.sort(intervals, (a,b)->(a[0]-b[0]));
        
        PriorityQueue<Integer> pq = new PriorityQueue<Integer>();
        pq.offer(intervals[0][1]);
        for(int i = 1; i < n; i++){
            if(intervals[i][0] >= pq.peek()) pq.poll();
            pq.offer(intervals[i][1]);
        }
        
        return pq.size();
    }
}
```

# Solution 2: Sort Start & End Seperately (Beat 100%)
```
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        int n = intervals.length;
        
        int[] start = new int[n];
        int[] end = new int[n];
        for(int i = 0; i < n; i++){
            start[i] =  intervals[i][0];
            end[i] =  intervals[i][1];
        }
        
        Arrays.sort(start);
        Arrays.sort(end);
        
        int room = 0, endIndex = 0;
        for(int i = 0; i < n; i++){
            if(start[i] < end[endIndex]) room++;
            else endIndex++;
        }
        
        return room;
    }
}
```