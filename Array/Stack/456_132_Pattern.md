# 456. 132 Pattern
Given an array of  `n`  integers  `nums`, a  **132 pattern**  is a subsequence of three integers  `nums[i]`,  `nums[j]`  and  `nums[k]`  such that  `i < j < k`  and  `nums[i] < nums[k] < nums[j]`.

Return  _`true`  if there is a  **132 pattern**  in  `nums`, otherwise, return  `false`._

**Example 1:**

**Input:** nums = [1,2,3,4]
**Output:** false
**Explanation:** There is no 132 pattern in the sequence.

**Example 2:**

**Input:** nums = [3,1,4,2]
**Output:** true
**Explanation:** There is a 132 pattern in the sequence: [1, 4, 2].

**Example 3:**

**Input:** nums = [-1,3,2,0]
**Output:** true
**Explanation:** There are three 132 patterns in the sequence: [-1, 3, 2], [-1, 3, 0] and [-1, 2, 0].

**Constraints:**

-   `n == nums.length`
-   `1 <= n <= 2 * 105`
-   `-109  <= nums[i] <= 109`

# Solution: Monotonic Decreasing Stack (Beat 100 %)
Refer from: https://leetcode.com/problems/132-pattern/discuss/94071/Single-pass-C%2B%2B-O(n)-space-and-time-solution-(8-lines)-with-detailed-explanation.
s3 是目前出现过最大的s3
```
class Solution {
    public boolean find132pattern(int[] nums) {
        int n = nums.length, index = -1, s3 = Integer.MIN_VALUE;
        // from right to left
        // s3 is the last one popped out from stack
        // If s3 exist, that means in stack, there must be at least one value that is greater than s3
        // and appears before s3
        // as long as s1 < s3, then we find a s1, s2, s3 where s1 < s3 < s2
        int[] stack = new int[n];
        
        for(int i = n-1; i >= 0; i--){
            int s1 = nums[i];
            if(s1 < s3) return true;
            while(index >= 0 && stack[index] < s1) s3 = stack[index--];
            stack[++index] = s1;
        }
        
        return false;
    }
}
```

@ye23 We can alsoleverage the input array to simulate a stack, so no extra space is required:
```
    public boolean find132pattern(int[] nums) {
        int two = Integer.MIN_VALUE;
        int index = nums.length;
        for (int i=nums.length-1; i>=0; i--) {
            if (nums[i] < two) return true;
            while (index < nums.length && nums[i] > nums[index]) {
                two = nums[index++];
            }
            nums[--index] = nums[i];
        }
        return false;
    }
```
    