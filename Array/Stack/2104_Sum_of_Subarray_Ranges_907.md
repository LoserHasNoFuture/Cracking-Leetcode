# 2104. Sum of Subarray Ranges 907
You are given an integer array  `nums`. The  **range**  of a subarray of  `nums`  is the difference between the largest and smallest element in the subarray.

Return  _the  **sum of all**  subarray ranges of_ `nums`_._

A subarray is a contiguous  **non-empty**  sequence of elements within an array.

**Example 1:**

**Input:** nums = [1,2,3]
**Output:** 4
**Explanation:** The 6 subarrays of nums are the following:
[1], range = largest - smallest = 1 - 1 = 0 
[2], range = 2 - 2 = 0
[3], range = 3 - 3 = 0
[1,2], range = 2 - 1 = 1
[2,3], range = 3 - 2 = 1
[1,2,3], range = 3 - 1 = 2
So the sum of all ranges is 0 + 0 + 0 + 1 + 1 + 2 = 4.

**Example 2:**

**Input:** nums = [1,3,3]
**Output:** 4
**Explanation:** The 6 subarrays of nums are the following:
[1], range = largest - smallest = 1 - 1 = 0
[3], range = 3 - 3 = 0
[3], range = 3 - 3 = 0
[1,3], range = 3 - 1 = 2
[3,3], range = 3 - 3 = 0
[1,3,3], range = 3 - 1 = 2
So the sum of all ranges is 0 + 0 + 0 + 2 + 0 + 2 = 4.

**Example 3:**

**Input:** nums = [4,-2,-3,4,1]
**Output:** 59
**Explanation:** The sum of all subarray ranges of nums is 59.

**Constraints:**

-   `1 <= nums.length <= 1000`
-   `-109  <= nums[i] <= 109`


# Solution : O(n^2)
```
class Solution {
    
    public long subArrayRanges(int[] nums) {
        int n = nums.length, min = 0, max = 0;
        long res = 0;
        
        for(int i = 0; i < n; i++){
            min = nums[i]; max = nums[i];
            for(int j = i+1; j < n; j++){
                min = Math.min(min, nums[j]);
                max = Math.max(max, nums[j]);
                
                res += (max - min);
            }
        }
        
        return res;
    }
    
}
```

# Solution 2: 907 Monotonic Stack O(n)
```
class Solution {
    
    public long subArrayRanges(int[] nums) {
        int n = nums.length;
        long res = 0;
        long[] min = new long[n], max = new long[n];
        
        Deque<Integer> stack = new ArrayDeque<Integer>();
        for(int i = 0; i < n; i++){
            while(!stack.isEmpty() && nums[stack.peek()] >= nums[i]){
                stack.pop();
            }
            
            if(!stack.isEmpty()) {
                min[i] = (long)(i - stack.peek()) * nums[i];
                min[i] += min[stack.peek()];
            }else min[i] = (long)(i + 1) * nums[i];
            res -= min[i];
            stack.push(i);
        }
        // System.out.println(Arrays.toString(min));
        
        stack = new ArrayDeque<Integer>();
        for(int i = 0; i < n; i++){
            while(!stack.isEmpty() && nums[stack.peek()] <= nums[i]){
                stack.pop();
            }
            if(!stack.isEmpty()) {
                max[i] = (long)(i - stack.peek()) * nums[i];
                max[i] += max[stack.peek()];
            }else max[i] = (long)(i + 1) * nums[i];
            res += max[i];
            stack.push(i);
        }
        // System.out.println(Arrays.toString(max));
        
        
        return res;
    }
    
}
```