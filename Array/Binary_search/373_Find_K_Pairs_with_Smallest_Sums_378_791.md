# 373. Find K Pairs with Smallest Sums
You are given two integer arrays  `nums1`  and  `nums2`  sorted in  **ascending order**  and an integer  `k`.

Define a pair  `(u, v)`  which consists of one element from the first array and one element from the second array.

Return  _the_  `k`  _pairs_  `(u1, v1), (u2, v2), ..., (uk, vk)`  _with the smallest sums_.

**Example 1:**

**Input:** nums1 = [1,7,11], nums2 = [2,4,6], k = 3
**Output:** [[1,2],[1,4],[1,6]]
**Explanation:** The first 3 pairs are returned from the sequence: [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]

**Example 2:**

**Input:** nums1 = [1,1,2], nums2 = [1,2,3], k = 2
**Output:** [[1,1],[1,1]]
**Explanation:** The first 2 pairs are returned from the sequence: [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]

**Example 3:**

**Input:** nums1 = [1,2], nums2 = [3], k = 3
**Output:** [[1,3],[2,3]]
**Explanation:** All possible pairs are returned from the sequence: [1,3],[2,3]

**Constraints:**

-   `1 <= nums1.length, nums2.length <= 104`
-   `-109  <= nums1[i], nums2[i] <= 109`
-   `nums1`  and  `nums2`  both are sorted in  **ascending order**.
-   `1 <= k <= 1000`


# Solution 1: Priority Queue (Beat 100%)
```
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        PriorityQueue<Tuple> pq = new PriorityQueue<Tuple>();
        int m = nums1.length, n = nums2.length;
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if(nums1 == null || nums1.length == 0 || nums2 == null || nums2.length == 0 || k <= 0) return res;
        
        for(int j = 0; j <= n-1; j++) pq.offer(new Tuple(0, j, nums1[0]+nums2[j]));
        for(int i = 0; i < Math.min(k, m *n); i++) {
            Tuple t = pq.poll();
            List<Integer> arr = new ArrayList<Integer>();
            arr.add(nums1[t.x]); arr.add(nums2[t.y]);
            res.add(arr);
            if(t.x == m - 1) continue;
            pq.offer(new Tuple (t.x + 1, t.y, nums1[t.x + 1] + nums2[t.y]));
        }
        return res;
    }
    

    class Tuple implements Comparable<Tuple> {
        int x, y, val;
        public Tuple (int x, int y, int val) {
            this.x = x;
            this.y = y;
            this.val = val;
        }

        @Override
        public int compareTo (Tuple that) {
            return this.val - that.val;
        }
    }
    
}
```


# Solution 2: Binary Search (Limited Time Exceed)
```
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        int m = nums1.length, n = nums2.length;
        int start = nums1[0]+nums2[0], end = nums1[m-1]+nums2[n-1];
        
        while(start < end){
            int mid = (start + end) >>> 1;
            
            int cnt = 0, j = m-1;
            for(int i = 0; i < n; i++){
                while(j >= 0 && nums1[j] > mid-nums2[i]) j--;
                cnt += j + 1;
            }
            
            if(cnt >= k) end = mid;
            else start = mid+1;
        }
        
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        get_results(nums1,nums2,res,start,k);
        
        return res;
    }
    
    
    public void get_results(int[] nums1, int[] nums2, List<List<Integer>> res, int target, int k){
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>((a,b)->(a[0]+a[1]-b[0]-b[1]));
        
        int end = nums1.length-1;
        for(int i = 0; i < nums2.length; i++){
            while(end >= 0 && nums1[end] > target-nums2[i]) end--;
            for(int j = 0; j <= end; j++) pq.offer(new int[]{nums1[j],nums2[i]});
        }
        
        while(!pq.isEmpty()){
            List<Integer> arr = new ArrayList<Integer>();
            int[] cur = pq.poll();
            arr.add(cur[0]);arr.add(cur[1]);
            res.add(arr);
            if(--k == 0) break;
        }
    }
    

}
```