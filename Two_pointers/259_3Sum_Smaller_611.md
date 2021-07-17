# 259. 3Sum Smaller
Given an array of  `n`  integers  `nums`  and an integer `target`, find the number of index triplets  `i`,  `j`,  `k`  with  `0 <= i < j < k < n`  that satisfy the condition  `nums[i] + nums[j] + nums[k] < target`.

**Example 1:**

**Input:** nums = [-2,0,1,3], target = 2
**Output:** 2
**Explanation:** Because there are two triplets which sums are less than 2:
[-2,0,1]
[-2,0,3]

**Example 2:**

**Input:** nums = [], target = 0
**Output:** 0

**Example 3:**

**Input:** nums = [0], target = 0
**Output:** 0

**Constraints:**

-   `n == nums.length`
-   `0 <= n <= 3500`
-   `-100 <= nums[i] <= 100`
-   `-100 <= target <= 100`

# Solution: O(n^2) 
```
public class Solution {
    public int threeSumSmaller(int[] nums, int target) {
        int result = 0;
        Arrays.sort(nums);
        for(int i = 0; i <= nums.length-3; i++) {
            int lo = i+1;
            int hi = nums.length-1;
            while(lo < hi) {
            // key!!!!
                if(nums[i] + nums[lo] + nums[hi] < target) {
                    result += hi - lo;
                    lo++;
                } else {
                    hi--;
                }
            }
        }
        return result;
    }
}
```