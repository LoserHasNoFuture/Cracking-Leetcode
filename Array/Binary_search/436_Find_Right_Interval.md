# 436. Find Right Interval
Given a set of intervals, for each of the interval i, check if there exists an interval j whose start point is bigger than or equal to the end point of the interval i, which can be called that j is on the "right" of i.

For any interval i, you need to store the minimum interval j's index, which means that the interval j has the minimum start point to build the "right" relationship for interval i. If the interval j doesn't exist, store -1 for the interval i. Finally, you need output the stored value of each interval as an array.

**Note:**

1.  You may assume the interval's end point is always bigger than its start point.
2.  You may assume none of these intervals have the same start point.

**Example 1:**

**Input:** [ [1,2] ]

**Output:** [-1]

**Explanation:** There is only one interval in the collection, so it outputs -1.

**Example 2:**

**Input:** [ [3,4], [2,3], [1,2] ]

**Output:** [-1, 0, 1]

**Explanation:** There is no satisfied "right" interval for [3,4].
For [2,3], the interval [3,4] has minimum-"right" start point;
For [1,2], the interval [2,3] has minimum-"right" start point.

**Example 3:**

**Input:** [ [1,4], [2,3], [3,4] ]

**Output:** [-1, 2, -1]

**Explanation:** There is no satisfied "right" interval for [1,4] and [3,4].
For [2,3], the interval [3,4] has minimum-"right" start point.

**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

# Solution 1: Priority Queue
```
class Solution {
    public int[] findRightInterval(int[][] intervals) {
        int len = intervals.length;
        int[] res = new int[len];
        if(len == 0) return res;
        
        PriorityQueue<int[]> start = new PriorityQueue<>((a,b)->a[0]-b[0]);
        PriorityQueue<int[]> end = new PriorityQueue<>((a,b)->a[0]-b[0]);
        
        for(int i = 0; i < len; i++) {
            start.offer(new int[] {intervals[i][0],i});
            end.offer(new int[] {intervals[i][1],i});
        }
        
        int[] cur_s = start.poll();
        int[] cur_e = end.poll();
        while(cur_e != null && cur_s != null){
            if(cur_e[0] <= cur_s[0]) {
                res[cur_e[1]] = cur_s[1];
                cur_e = end.poll();
            }else if(!start.isEmpty()) cur_s = start.poll();
            else cur_s = null;
        }
        
        while(cur_e != null){
            res[cur_e[1]] = -1;
            if(!end.isEmpty()) cur_e = end.poll();
            else cur_e = null;
        }
        
        
        return res;
    }
}
```


# Solution 2: Calculating Score
Inspired by one solution of 1337:[https://leetcode.com/problems/the-k-weakest-rows-in-a-matrix/discuss/560163/Simple-Solution-In-Java](https://leetcode.com/problems/the-k-weakest-rows-in-a-matrix/discuss/560163/Simple-Solution-In-Java)

We calculate score in a similar way. But special trick needs to be used to deal with negative numbers.

The only draw back of this code is that it has the risk of overflow.

See my code below:

```
class Solution {
    public int[] findRightInterval(int[][] intervals) {
        int len = intervals.length;
        int[] res = new int[len];
        if(len == 0) return res;
        
        int[] score = new int[len];
		// score is calculated as following 
        for(int i = 0; i < len; i++) score[i] =intervals[i][0] * len + i;  

        Arrays.sort(score);
        
        for(int i = 0; i < len; i++){
            res[i] = find_position(score, intervals[i][1]);
        }    
        
        return res;
    }
    
    public int find_position(int[] nums, int target){
        int start = 0;
        int end = nums.length;
        int len = nums.length;
        while(start < end){
            int mid = (start + end) >>> 1;
            int val = nums[mid] >= 0? nums[mid]/len : nums[mid]/len-1;
            if(val < target) start = mid + 1;
            else end = mid;
           
        }
        
        if(start == len) return -1;
        else return nums[start] >= 0? nums[start]%len:nums[start]%len + len;
    }
}
```

