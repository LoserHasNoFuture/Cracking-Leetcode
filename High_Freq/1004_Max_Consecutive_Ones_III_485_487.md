# 1004. Max Consecutive Ones III 485 487

Given a binary array  `nums`  and an integer  `k`, return  _the maximum number of consecutive_ `1`_'s in the array if you can flip at most_  `k`  `0`'s.

**Example 1:**

**Input:** nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
**Output:** 6
**Explanation:** [1,1,1,0,0,**1**,1,1,1,1,**1**]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**Example 2:**

**Input:** nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
**Output:** 10
**Explanation:** [0,0,1,1,**1**,**1**,1,1,1,**1**,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `nums[i]`  is either  `0`  or  `1`.
-   `0 <= k <= nums.length`


# Solution 1: Sliding Window (Beat 100%)
```
class Solution {
    public int longestOnes(int[] nums, int k) {
        int left = 0, right = 0, max = 0, cnt = 0;;
        
        while(right < nums.length){
            
            if (nums[right] == 0 && cnt < k) {
                cnt++;
            }else if (nums[right] == 0 && cnt == k){
                while(cnt == k){
                    if(nums[left] == 0) cnt--;
                    left++;
                }
                cnt++;
            }
            right++;
            max = Math.max(max,right-left);
            
        }
        
        return max;
    }
}
```