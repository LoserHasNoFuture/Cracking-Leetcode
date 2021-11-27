# 238. Product of Array Except Self
Given an integer array  `nums`, return  _an array_  `answer`  _such that_  `answer[i]`  _is equal to the product of all the elements of_  `nums`  _except_  `nums[i]`.

The product of any prefix or suffix of  `nums`  is  **guaranteed**  to fit in a  **32-bit**  integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

**Example 1:**

**Input:** nums = [1,2,3,4]
**Output:** [24,12,8,6]

**Example 2:**

**Input:** nums = [-1,1,0,-3,3]
**Output:** [0,0,9,0,0]

**Constraints:**

-   `2 <= nums.length <= 105`
-   `-30 <= nums[i] <= 30`
-   The product of any prefix or suffix of  `nums`  is  **guaranteed**  to fit in a  **32-bit**  integer.

**Follow up:** Can you solve the problem in  `O(1)` extra space complexity? (The output array  **does not**  count as extra space for space complexity analysis.)

# Solution
```
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] res = new int[n];
        
        for(int i = 0; i < n-1; i++){
            if(i == 0) res[i] = nums[i];
            else res[i] = res[i-1]*nums[i];
        }
        
        int post = 1;
        for(int i = n-1; i >= 0; i--){
            if(i == 0) res[i] = post;
            else{
                res[i] = res[i-1]*post;
                post = post*nums[i];
            }
        }
        
        return res;
    }
}
```