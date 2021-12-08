# 259. 3Sum Smaller 611
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
class Solution {
    int res = 0, n = 0, r = 0;
    public int threeSumSmaller(int[] nums, int target) {
        if(nums.length < 3) return 0;
        Arrays.sort(nums);
        
        n = nums.length; r = n - 1;
        for(int i = 0; i < n-2; i++){
            two_sum(nums, i + 1, n - 1, target-nums[i]);
        }
        
        return res;
    }
    
    public void two_sum(int[] nums, int left, int right, int target){
        right = r; r = 0;
        
        while(left < right){
            if(nums[left] + nums[right] >= target) right--;
            else {
                r = Math.max(r, right);
                res += right - left;
                left++;
            }
        }
        
    }
}
```

