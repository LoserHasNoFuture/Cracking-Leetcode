# 189. Rotate Array
Given an array, rotate the array to the right by  `k`  steps, where  `k`  is non-negative.

**Example 1:**

**Input:** nums = [1,2,3,4,5,6,7], k = 3
**Output:** [5,6,7,1,2,3,4]
**Explanation:**
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]

**Example 2:**

**Input:** nums = [-1,-100,3,99], k = 2
**Output:** [3,99,-1,-100]
**Explanation:** 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]

**Constraints:**

-   `1 <= nums.length <= 105`
-   `-231  <= nums[i] <= 231  - 1`
-   `0 <= k <= 105`

**Follow up:**

-   Try to come up with as many solutions as you can. There are at least  **three**  different ways to solve this problem.
-   Could you do it in-place with  `O(1)`  extra space?

# Solution: Reverse 3 times
```
class Solution {
    public void rotate(int[] nums, int k) {
        k %= nums.length;
        reverse(nums, 0, nums.length - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, nums.length - 1);
    }

    public void reverse(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
}
```

# Solution: Move n times
```
class Solution {
    public void rotate(int[] nums, int k) {
        if(k == 0) return;
        int n = nums.length, start = 0, cnt = n;
        k = k%n;
        
        while(cnt > 0){
            int ptr = start, tmp = nums[ptr];
            do{
                int next = (ptr + k)%n;
                int val = nums[next];
                nums[next] = tmp;
                tmp = val;
                ptr = next;
                cnt--;
            }while(ptr != start && cnt > 0);
            
            start++;
        }
    }
}
```

# Solution: Other ways
https://leetcode.com/problems/rotate-array/discuss/54277/Summary-of-C%2B%2B-solutions
