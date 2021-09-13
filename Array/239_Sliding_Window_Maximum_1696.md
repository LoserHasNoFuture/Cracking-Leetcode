# 239. Sliding Window Maximum 1696
You are given an array of integers `nums`, there is a sliding window of size  `k`  which is moving from the very left of the array to the very right. You can only see the  `k`  numbers in the window. Each time the sliding window moves right by one position.

Return  _the max sliding window_.

**Example 1:**

**Input:** nums = [1,3,-1,-3,5,3,6,7], k = 3
**Output:** [3,3,5,5,6,7]
**Explanation:** 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       **3**
 1 [3  -1  -3] 5  3  6  7       **3**
 1  3 [-1  -3  5] 3  6  7      ** 5**
 1  3  -1 [-3  5  3] 6  7       **5**
 1  3  -1  -3 [5  3  6] 7       **6**
 1  3  -1  -3  5 [3  6  7]      **7**

**Example 2:**

**Input:** nums = [1], k = 1
**Output:** [1]

**Example 3:**

**Input:** nums = [1,-1], k = 1
**Output:** [1,-1]

**Example 4:**

**Input:** nums = [9,11], k = 2
**Output:** [11]

**Example 5:**

**Input:** nums = [4,-2], k = 2
**Output:** [4]

**Constraints:**

-   `1 <= nums.length <= 105`
-   `-104  <= nums[i] <= 104`
-   `1 <= k <= nums.length`

# Solution 1: Deque O(n)
```
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] res = new int[n-k+1];
        Deque<int[]> deque = new ArrayDeque<int[]>();
        
        for(int i = 0; i < n; i++){
            while(!deque.isEmpty() && i-deque.getFirst()[1] >= k) deque.pollFirst();
            while(!deque.isEmpty() && deque.getLast()[0] < nums[i]) deque.pollLast();
            
            deque.addLast(new int[]{nums[i],i});
            if(i >= k-1) res[i-k+1] = deque.getFirst()[0];
        }
        
        return res;
    }
}
```