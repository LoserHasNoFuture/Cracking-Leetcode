# 368. Largest Divisible Subset
Given a set of  **distinct**  positive integers  `nums`, return the largest subset  `answer`  such that every pair  `(answer[i], answer[j])`  of elements in this subset satisfies:

-   `answer[i] % answer[j] == 0`, or
-   `answer[j] % answer[i] == 0`

If there are multiple solutions, return any of them.

**Example 1:**

**Input:** nums = [1,2,3]
**Output:** [1,2]
**Explanation:** [1,3] is also accepted.

**Example 2:**

**Input:** nums = [1,2,4,8]
**Output:** [1,2,4,8]

**Constraints:**

-   `1 <= nums.length <= 1000`
-   `1 <= nums[i] <= 2 * 109`
-   All the integers in  `nums`  are  **unique**.

# Solution: Dynamic Programming
```
class Solution {
    public List<Integer> largestDivisibleSubset(int[] nums) {
        List<Integer> res = new ArrayList<Integer>();
        int n = nums.length, max = 1, last = 1;
//			This is a one dimensional DP        
//         first element is cnt (dp part), second element is the last one (extra space for saving previous element)
        int[][] dp = new int[n+1][2];
        Arrays.sort(nums);
        
        dp[1][0] = 1; 
        for(int i = 1; i < n; i++){
            dp[i+1][0] = 1;
            for(int j = 0; j < i; j++){
                if(nums[i]%nums[j] == 0){
                    if(dp[j+1][0] + 1 > dp[i+1][0]){
                        dp[i+1][0] = dp[j+1][0] + 1;
                        dp[i+1][1] = j+1;
                    }
                }
            }
            
            if(dp[i+1][0] > max){
                max = dp[i+1][0];
                last = i+1;
            }
        }
        
        while(last > 0){
            int index = last - 1;
            res.add(0,nums[index]);
            last = dp[last][1];
        }
        
        return res;
    }
}
```