# 300. Longest Increasing Subsequence
Given an integer array  `nums`, return the length of the longest strictly increasing subsequence.

A  **subsequence**  is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example,  `[3,6,2,7]`  is a subsequence of the array  `[0,3,1,6,2,2,7]`.

**Example 1:**

**Input:** nums = [10,9,2,5,3,7,101,18]
**Output:** 4
**Explanation:** The longest increasing subsequence is [2,3,7,101], therefore the length is 4.

**Example 2:**

**Input:** nums = [0,1,0,3,2,3]
**Output:** 4

**Example 3:**

**Input:** nums = [7,7,7,7,7,7,7]
**Output:** 1

**Constraints:**

-   `1 <= nums.length <= 2500`
-   `-104  <= nums[i] <= 104`

**Follow up:** Can you come up with an algorithm that runs in `O(n log(n))`  time complexity?

# Solution 1: DP, O(n^2), (Beat 56%)
```
class Solution {
    public int lengthOfLIS(int[] nums) {
        if(nums.length == 0) return 0;
        int[] dp = new int[nums.length];
        Arrays.fill(dp,1);
        
        int max = 1;
        for(int i = 1; i < nums.length; i++){
            for(int j = 0; j < i; j++){
                if(nums[i] > nums[j]) dp[i] = Math.max(dp[i], dp[j] + 1);
            }
            max = Math.max(max,dp[i]);
        }
        
        return max;
    }
}
```

# Solution 2: Patience Sorting, O(nlogn) (Beat 100%)
```
class Solution {
    public int lengthOfLIS(int[] nums) {
        if(nums.length == 0 || nums.length == 1) return nums.length;
        int[] piles = new int[nums.length];
        piles[0] = nums[0];
        int index = 0; 
        
        for(int i = 1; i < nums.length; i++){
            int next = find_insert_postion(piles,index,nums[i]);
            if(next > index) index = next;
            piles[next] = nums[i];
        }
        
        return index + 1;
    }
    
    public int find_insert_postion(int[] piles, int end, int target){
        int start = 0;
        while(start < end){
            int mid = (start + end) >>> 1;
            if(piles[mid] == target) return mid;
            if(piles[mid] < target) start = mid + 1;
            else end = mid;
        }
        return piles[start] >= target? start: start+1;
    }
}
```