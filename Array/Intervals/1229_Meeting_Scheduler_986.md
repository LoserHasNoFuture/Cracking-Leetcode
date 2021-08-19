# 1229. Meeting Scheduler
Given the availability time slots arrays  `slots1`  and  `slots2`  of two people and a meeting duration  `duration`, return the  **earliest time slot**  that works for both of them and is of duration  `duration`.

If there is no common time slot that satisfies the requirements, return an  **empty array**.

The format of a time slot is an array of two elements  `[start, end]`  representing an inclusive time range from  `start`  to  `end`.

It is guaranteed that no two availability slots of the same person intersect with each other. That is, for any two time slots  `[start1, end1]`  and  `[start2, end2]`  of the same person, either  `start1 > end2`  or  `start2 > end1`.

**Example 1:**

**Input:** slots1 = [[10,50],[60,120],[140,210]], slots2 = [[0,15],[60,70]], duration = 8
**Output:** [60,68]

**Example 2:**

**Input:** slots1 = [[10,50],[60,120],[140,210]], slots2 = [[0,15],[60,70]], duration = 12
**Output:** []

**Constraints:**

-   `1 <= slots1.length, slots2.length <= 104`
-   `slots1[i].length, slots2[i].length == 2`
-   `slots1[i][0] < slots1[i][1]`
-   `slots2[i][0] < slots2[i][1]`
-   `0 <= slots1[i][j], slots2[i][j] <= 109`
-   `1 <= duration <= 106`

# Solution 1: O(nlogn + mlogm)
```
class Solution {
    public List<Integer> minAvailableDuration(int[][] slots1, int[][] slots2, int duration) {
        Arrays.sort(slots1, (a,b)->(a[0]-b[0]));
        Arrays.sort(slots2, (a,b)->(a[0]-b[0]));
        int ptr1 = 0, ptr2 = 0, m = slots1.length, n = slots2.length;
        List<Integer> res = new ArrayList<Integer>();
        
        while(ptr1 < m && ptr2 < n){
            
            int start = Math.max(slots1[ptr1][0],slots2[ptr2][0]);
            int end = Math.min(slots1[ptr1][1],slots2[ptr2][1]);
            if(end - start >= duration){
                res.add(start);
                res.add(start+duration);
                break;
            }

            if(slots1[ptr1][1] < slots2[ptr2][1]) ptr1++;
            else ptr2++;

        }
        
        return res;
    }
}
```

# Solution 2: O((m+n)log(m+n))
```
class Solution {
    public List<Integer> minAvailableDuration(int[][] slots1, int[][] slots2, int duration) {
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>((a,b)->(a[0]-b[0]));
        List<Integer> res = new ArrayList<Integer>();
        
        for(int[] slot: slots1){
            if(slot[1] - slot[0] >= duration) pq.offer(slot);
        }
        for(int[] slot: slots2){
            if(slot[1] - slot[0] >= duration) pq.offer(slot);
        }
        
        while(!pq.isEmpty()){
            int[] cur = pq.poll();
            if(pq.isEmpty()) break;
            if(cur[1] >= pq.peek()[0] + duration){
                res.add(pq.peek()[0]);
                res.add(pq.peek()[0] + duration);
                break;
            }
        }
        
        return res;
    }
}
```