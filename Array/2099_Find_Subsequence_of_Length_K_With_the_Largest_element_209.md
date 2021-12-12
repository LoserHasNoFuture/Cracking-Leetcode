# 2099. Find Subsequence of Length K With the Largest Sum
You are given an integer array  `nums`  and an integer  `k`. You want to find a  **subsequence** of  `nums`  of length  `k`  that has the  **largest**  sum.

Return  _**any**  such subsequence as an integer array of length_ `k`.

A  **subsequence**  is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** nums = [2,1,3,3], k = 2
**Output:** [3,3]
**Explanation:**
The subsequence has the largest sum of 3 + 3 = 6.

**Example 2:**

**Input:** nums = [-1,-2,3,4], k = 3
**Output:** [-1,3,4]
**Explanation:** 
The subsequence has the largest sum of -1 + 3 + 4 = 6.

**Example 3:**

**Input:** nums = [3,4,3,3], k = 2
**Output:** [3,4]
**Explanation:**
The subsequence has the largest sum of 3 + 4 = 7. 
Another possible subsequence is [4, 3].

**Constraints:**

-   `1 <= nums.length <= 1000`
-   `-105 <= nums[i] <= 105`
-   `1 <= k <= nums.length`

# Solution 1: Sorting O(nlogn)
```
class Solution {
    public int[] maxSubsequence(int[] nums, int k) {
        int n = nums.length;
        int[][] arr = new int[n][2], res = new int[k][2];
        for(int i = 0; i < n; i++) {
            arr[i][0] = nums[i];
            arr[i][1] = i;
        }
        Arrays.sort(arr, (a,b)->(b[0]-a[0]));
        
        for(int i = 0; i < k; i++) res[i] = arr[i];
        Arrays.sort(res,(a,b)->(a[1]-b[1]));
        
        int[] fin = new int[k];
        for(int i = 0; i < k; i++) fin[i] = res[i][0];
        
        return fin;
    }
}
```

# Solution 2: Using PQ O(klogk)

# Solution 3:
1. Using quick selection to find the k-th largest element 
2. Sweep all elements, add the elments that are >= k  into res
3. Check while res.size() >= k, delete kth largest element