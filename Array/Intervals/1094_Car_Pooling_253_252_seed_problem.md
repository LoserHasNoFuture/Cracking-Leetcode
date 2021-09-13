# 1094. Car Pooling 253
There is a car with  `capacity`  empty seats. The vehicle only drives east (i.e., it cannot turn around and drive west).

You are given the integer  `capacity`  and an array  `trips`  where  `trip[i] = [numPassengersi, fromi, toi]`  indicates that the  `ith`  trip has  `numPassengersi`  passengers and the locations to pick them up and drop them off are  `fromi`  and  `toi`  respectively. The locations are given as the number of kilometers due east from the car's initial location.

Return  `true` _if it is possible to pick up and drop off all passengers for all the given trips, or_ `false` _otherwise_.

**Example 1:**

**Input:** trips = [[2,1,5],[3,3,7]], capacity = 4
**Output:** false

**Example 2:**

**Input:** trips = [[2,1,5],[3,3,7]], capacity = 5
**Output:** true

**Example 3:**

**Input:** trips = [[2,1,5],[3,5,7]], capacity = 3
**Output:** true

**Example 4:**

**Input:** trips = [[3,2,7],[3,7,9],[8,3,9]], capacity = 11
**Output:** true

**Constraints:**

-   `1 <= trips.length <= 1000`
-   `trips[i].length == 3`
-   `1 <= numPassengersi  <= 100`
-   `0 <= fromi  < toi  <= 1000`
-   `1 <= capacity <= 105`

# Solution 1: Sort Intervals + PriorityQueue O(n^2logn)
```
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {
        if(trips.length == 0) return true;
        Arrays.sort(trips, (a,b)->(a[1]-b[1]));
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>((a,b)->(a[2]-b[2]));
        int cnt = trips[0][0];
        if(cnt > capacity) return false;
        pq.offer(trips[0]);
        
        for(int i = 1; i < trips.length; i++){
            while(!pq.isEmpty() && trips[i][1] >= pq.peek()[2]) cnt -= pq.poll()[0];
            pq.offer(trips[i]);
            cnt += trips[i][0];
            if(cnt > capacity) return false;
        }
        
        return true;
    }
}
```

# Solution 2: Sort start & end seperately O(nlogn)
```
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {
        if(trips.length == 0) return true;
        int n = trips.length, index = 0;
        int[][] start = new int[n][2], end = new int[n][2];
        for(int[] trip: trips){
            start[index] = new int[]{trip[1],trip[0]};
            end[index] = new int[]{trip[2],trip[0]};
            index++;
        }
        Arrays.sort(start, (a,b)->(a[0]-b[0]));
        Arrays.sort(end, (a,b)->(a[0]-b[0]));
        
        int cnt = 0, ptrs = 0, ptre = 0;
        while(ptrs < n && ptre < n){
            if(start[ptrs][0] < end[ptre][0]){
                cnt += start[ptrs][1];
                ptrs++;
            }else{
                cnt -= end[ptre][1];
                ptre++;
            }
            if(cnt > capacity) return false;
        }

        return true;
    }
}
```

```
// see illustration in 253. meeting rooms II
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {
        int cnt = 0, n = trips.length, index = 0;
        int[][] start = new int[n][2], end = new int[n][2];
        for(int[] trip: trips){
            start[index] = new int[]{trip[1],trip[0]};
            end[index++] = new int[]{trip[2],-trip[0]};
        }
        Arrays.sort(start,(a,b)->(a[0]-b[0]));
        Arrays.sort(end,(a,b)->(a[0]-b[0]));
        
        int endIndex = 0;
        for(int i = 0; i < n; i++){
            while(endIndex < n && start[i][0] >= end[endIndex][0]){
                cnt += end[endIndex][1];
                endIndex++;
            }
            cnt += start[i][1];
            if(cnt > capacity) return false;
        }

        return true;
    }
}
```


# Solution 4: Sweeping Line O(nlogn)
```
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {
        int cnt = 0, n = trips.length, index = 0;
        int[][] nums = new int[n*2][2];
        for(int[] trip:trips){
            nums[index++] = new int[]{trip[1], trip[0]};
            nums[index++] = new int[]{trip[2], -trip[0]};
        }
        
        // can use bucket sort -> lead to solution 2
        Arrays.sort(nums, (a,b)->(a[0]==b[0]?a[1]-b[1]:a[0]-b[0]));
        
        for(int[] num:nums){
            cnt += num[1];
            if(cnt > capacity) return false;
        }
        
        return true;
    }
}
```

```
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {
        int cnt = 0, n = trips.length;
        int[] bucket = new int[1001];
        for(int[] trip:trips){
            bucket[trip[1]] += trip[0];
            bucket[trip[2]] -= trip[0];
        }
        
        for(int i = 0; i < 1001; i++){
            cnt += bucket[i];
            if(cnt > capacity) return false;
        }

        return true;
    }
}
```