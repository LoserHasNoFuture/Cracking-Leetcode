# 362. Design Hit Counter 1348
Design a hit counter which counts the number of hits received in the past  `5`  minutes (i.e., the past  `300`  seconds).

Your system should accept a  `timestamp`  parameter (**in seconds**  granularity), and you may assume that calls are being made to the system in chronological order (i.e.,  `timestamp`  is monotonically increasing). Several hits may arrive roughly at the same time.

Implement the  `HitCounter`  class:

-   `HitCounter()`  Initializes the object of the hit counter system.
-   `void hit(int timestamp)`  Records a hit that happened at  `timestamp`  (**in seconds**). Several hits may happen at the same  `timestamp`.
-   `int getHits(int timestamp)`  Returns the number of hits in the past 5 minutes from  `timestamp`  (i.e., the past  `300`  seconds).

**Example 1:**

**Input**
["HitCounter", "hit", "hit", "hit", "getHits", "hit", "getHits", "getHits"]
[[], [1], [2], [3], [4], [300], [300], [301]]
**Output**
[null, null, null, null, 3, null, 4, 3]

**Explanation**
HitCounter hitCounter = new HitCounter();
hitCounter.hit(1);       // hit at timestamp 1.
hitCounter.hit(2);       // hit at timestamp 2.
hitCounter.hit(3);       // hit at timestamp 3.
hitCounter.getHits(4);   // get hits at timestamp 4, return 3.
hitCounter.hit(300);     // hit at timestamp 300.
hitCounter.getHits(300); // get hits at timestamp 300, return 4.
hitCounter.getHits(301); // get hits at timestamp 301, return 3.

**Constraints:**

-   `1 <= timestamp <= 2 * 109`
-   All the calls are being made to the system in chronological order (i.e.,  `timestamp`  is monotonically increasing).
-   At most  `300`  calls will be made to  `hit`  and  `getHits`.

**Follow up:**  What if the number of hits per second could be huge? Does your design scale?

# Solution 1: Binary Search (Beat 100%)
```
class HitCounter {
    
    class Hit{
        int timestamp;
        int cnt;
        
        public Hit(int _timestamp, int _cnt){
            this.timestamp = _timestamp;
            this.cnt = _cnt;
        }
    }

    /** Initialize your data structure here. */
    ArrayList<Hit> arr;
    public HitCounter() {
        arr = new ArrayList<Hit>();
    }
    
    /** Record a hit.
        @param timestamp - The current timestamp (in seconds granularity). */
    public void hit(int timestamp) {
        if(arr.size() > 0){
            Hit hit = arr.get(arr.size()-1);
            if(hit.timestamp == timestamp) hit.cnt++;
            else arr.add(new Hit(timestamp,1));
        }else arr.add(new Hit(timestamp,1));
    }
    
    /** Return the number of hits in the past 5 minutes.
        @param timestamp - The current timestamp (in seconds granularity). */
    public int getHits(int timestamp) {
        if(arr.size() == 0) return 0;
        int first = find_first_index(timestamp-299);
        if(first == -1) return 0;
        int last = find_last_index(timestamp,first);
        
        int res = 0;
        while(first < arr.size() && first <= last){
            res += arr.get(first).cnt;
            first++;
        }
        
        return res;
    }
    
//     larger than or equal to
    public int find_first_index(int target){
        int start = 0, end = arr.size() - 1;
        while(start < end){
            int mid = (start + end) >>> 1;
            if(arr.get(mid).timestamp == target) return mid;
            if(arr.get(mid).timestamp > target) end = mid;
            else start = mid + 1;
        }
        return arr.get(start).timestamp >= target? start: -1;
    }
    
//     smaller than or equal to
    public int find_last_index(int target, int start){
        int end = arr.size() - 1;
        while(start < end){
            int mid = (start + end + 1) >>> 1;
            if(arr.get(mid).timestamp == target) return mid;
            if(arr.get(mid).timestamp > target) end = mid - 1;
            else start = mid;
        }
        return start;
    }
}
```