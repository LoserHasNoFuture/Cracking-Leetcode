# 719. Find K-th Smallest Pair Distance
The  **distance of a pair**  of integers  `a`  and  `b`  is defined as the absolute difference between  `a`  and  `b`.

Given an integer array  `nums`  and an integer  `k`, return  _the_  `kth`  _smallest  **distance among all the pairs**_  `nums[i]`  _and_  `nums[j]`  _where_  `0 <= i < j < nums.length`.

**Example 1:**

**Input:** nums = [1,3,1], k = 1
**Output:** 0
**Explanation:** Here are all the pairs:
(1,3) -> 2
(1,1) -> 0
(3,1) -> 2
Then the 1st smallest distance pair is (1,1), and its distance is 0.

**Example 2:**

**Input:** nums = [1,1,1], k = 2
**Output:** 0

**Example 3:**

**Input:** nums = [1,6,1], k = 3
**Output:** 5

**Constraints:**

-   `n == nums.length`
-   `2 <= n <= 104`
-   `0 <= nums[i] <= 106`
-   `1 <= k <= n * (n - 1) / 2`

# Solution: Heap (Limited Time Exceed)
```
class Solution {
    public int smallestDistancePair(int[] nums, int k) {
        Arrays.sort(nums);
        PriorityQueue<Node> pq = new PriorityQueue<Node>();
        for(int i = 0; i < nums.length-1; i++) pq.offer(new Node(i,i+1,nums[i+1]-nums[i]));
        
        while(!pq.isEmpty()){
            Node cur = pq.poll();
            if(--k == 0) return cur.val;
            
            if(cur.y + 1 < nums.length) 
                pq.offer(new Node(cur.x, cur.y+1, nums[cur.y+1]-nums[cur.x]));
        }
        
        return 0;
    }
    
    class Node implements Comparable<Node>{
        int x;
        int y;
        int val;
        
        public Node (int _x, int _y, int _val){
            this.x = _x;
            this.y = _y;
            this.val = _val;
        }
        
        @Override
        public int compareTo(Node that){
            return this.val - that.val;
        }
    }
}
```

# Solution 2: Binary Search 
(Beat 28%)
```
class Solution {

    public int smallestDistancePair(int[] nums, int k) {
        int n = nums.length;
        Arrays.sort(nums);

        // Minimum absolute difference
        int start = Integer.MAX_VALUE;
        for (int i = 1; i < n; i++)
            start = Math.min(start, nums[i] - nums[i-1]);

        // Maximum absolute difference
        int end = nums[n - 1] - nums[0];

        while(start < end){
            int mid = (start + end) >>> 1;
            
            if(smaller_than(k, mid, nums)) start = mid+1;
            else end = mid;
            
        }
        
        return start;
    }
    
    public boolean smaller_than(int k, int target, int[] nums){
        int n = nums.length, cnt = 0;
        
        for(int i = 0; i < n - 1; i++){
            cnt += cnt_num_of_pairs(nums, i+1, target+nums[i]);
            if(cnt >= k) return false;
        }
        
        return true;
    }
    
    // num of elements that are smaller than or equal to target
    public int cnt_num_of_pairs(int[] nums, int xx, int target){
        int start = xx - 1, end = nums.length-1;
        
        while(start < end){
            int mid = (start + end + 1) >>> 1;
            
            if(nums[mid] <= target)  start = mid;
            else end = mid - 1;
        }
        
        return start - xx + 1;
    }
}
```

A lot better coding skill: (Beat 35%)
Refer from: [https://leetcode.com/problems/find-k-th-smallest-pair-distance/discuss/109094/Java-very-Easy-and-Short(15-lines-Binary-Search-and-Bucket-Sort)-solutions](https://leetcode.com/problems/find-k-th-smallest-pair-distance/discuss/109094/Java-very-Easy-and-Short(15-lines-Binary-Search-and-Bucket-Sort)-solutions)
```
class Solution {
    public int smallestDistancePair(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length, low = 0, hi = nums[n-1] - nums[0];
        while (low < hi) {
            int cnt = 0, j = 0, mid = (low + hi)/2;
            for (int i = 0; i < n; ++i) {
                while (j < n && nums[j] - nums[i] <= mid) ++j;
                cnt += j - i-1;
            }
            if (cnt >= k) 
                hi = mid;
            
            else low = mid + 1;
        }
        
        return low;
    }
}
```