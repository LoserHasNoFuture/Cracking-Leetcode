# 53. Maximum Subarray
Given an integer array  `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return  _its sum_.

**Example 1:**

**Input:** nums = [-2,1,-3,4,-1,2,1,-5,4]
**Output:** 6
**Explanation:** [4,-1,2,1] has the largest sum = 6.

**Example 2:**

**Input:** nums = [1]
**Output:** 1

**Example 3:**

**Input:** nums = [5,4,-1,7,8]
**Output:** 23

**Constraints:**

-   `1 <= nums.length <= 3 * 104`
-   `-105  <= nums[i] <= 105`

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution using the **divide and conquer** approach, which is more subtle.

# Solution 1: Direct Solution (DP)
```
class Solution {
    public int maxSubArray(int[] nums) {
        if(nums.length == 0) return 0;
        int max = nums[0], sum = nums[0];
        
        for(int i = 1; i < nums.length; i++){
            if(sum < 0) sum = nums[i];
            else sum += nums[i];
            
            if(sum > max) max = sum;
        }
        
        return max;
    }
}
```

# Solution 2: Divide and Conquer 
f(n) = 2f(n/2) + O(1) --> O(n)

```
class Solution {
    public int maxSubArray(int[] nums) {

        // use divide and conquer: store current max sum of left-attached
        // subarray, max sum of right-attached subarray, max sum, and sum
        // Time: O(n) because f(n) = 2 * f(n/2) + O(1) => f(n)=O(n)
        // Space: O(lgn) because the runtime stack is of maximum depth O(lgn)


        int n = nums.length;
        if (n == 0)
            return 0;

        return helper(0, n - 1, nums)[2];

    }

    private int[] helper(int left, int right, int[] nums) {
        // return values: leftMax, rightMax, max, sum

        if (left == right)
            return new int[]{nums[left], nums[right], nums[left], nums[left]};

        int mid = (left + right) / 2;
        int[] leftSums = helper(left, mid, nums),
              rightSums = helper(mid + 1, right, nums);

        return new int[]{Math.max(leftSums[0], leftSums[3] + rightSums[0]),
                         Math.max(rightSums[1], rightSums[3] + leftSums[1]),
                         Math.max(leftSums[1] + rightSums[0], 
                                  Math.max(leftSums[2], rightSums[2])),
                         leftSums[3] + rightSums[3]};

    }
}
```