# 416. Partition Equal Subset Sum 474 1049 494
Given a  **non-empty**  array  `nums`  containing  **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

**Example 1:**

**Input:** nums = [1,5,11,5]
**Output:** true
**Explanation:** The array can be partitioned as [1, 5, 5] and [11].

**Example 2:**

**Input:** nums = [1,2,3,5]
**Output:** false
**Explanation:** The array cannot be partitioned into equal sum subsets.

**Constraints:**

-   `1 <= nums.length <= 200`
-   `1 <= nums[i] <= 100`


# Solution 1: 0-1 KnapSack (Beat 75%)
```
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        
        for(int num: nums) sum += num;
        
        if(sum%2 == 1) return false;
        
        return subsets(nums,sum/2);
    }
    
    public boolean subsets(int[] nums, int target){
        int n = nums.length;
        // Can also use boolean
        int[] dp = new int[target+1];
        
        dp[0] = 1;
        
        for(int i = 1; i <= n; i++){
            int[] next = new int[target+1];
            for(int j = 0; j <= target; j++){
                next[j] = dp[j] + (j>= nums[i-1]? dp[j-nums[i-1]]:0);                
            }
            if(next[target] > 0) return true;
            dp = next;
        }
        
        return false;
    }
}
```