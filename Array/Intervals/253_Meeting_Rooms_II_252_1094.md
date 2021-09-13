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
        int n = intervals.length, index = 0, rooms = 0;
        int[] start = new int[n], end = new int[n];
        for(int[] cur: intervals){
            start[index] = cur[0];
            end[index++] = cur[1];
        }
        Arrays.sort(start);
        Arrays.sort(end);
        
        int endIndex = 0;
        for(int i = 0; i < n; i++){
            if(start[i] < end[endIndex]) rooms++;
            else endIndex++;
        }
        
        return rooms;
        
        
        // [[2,15],[36,45],[9,29],[16,23],[4,9]]
        // start:  2 4 9 16 36
        // end: 9 15 23 29 45
        // start  end   rooms  endIndex
        //   2  <  9    0->1      0
        //   4  <  9    1->2      0
        //   16 >= 9     2       0->1
        //   36 >= 15    2       1->2
    }
}
```

# Solution 3: Sweeping Line O(nlogn)
```
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        int n = intervals.length, index = 0, res = 0, cnt = 0;
        int[][] nums = new int[2*n][2];
        for(int[] cur: intervals){
            nums[index++] = new int[]{cur[0], 1};
            nums[index++] = new int[]{cur[1], -1};
        }
        Arrays.sort(nums, (a,b)->(a[0]==b[0]?a[1]-b[1]:a[0]-b[0]));
        
        for(int[] num: nums){
            cnt += num[1];
            res = Math.max(res, cnt);
        }
        
        return res;
    }
}
```