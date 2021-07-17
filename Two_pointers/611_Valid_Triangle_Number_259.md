# 611. Valid Triangle Number 259
Given an integer array  `nums`, return  _the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle_.

**Example 1:**

**Input:** nums = [2,2,3,4]
**Output:** 3
**Explanation:** Valid combinations are: 
2,3,4 (using the first 2)
2,3,4 (using the second 2)
2,2,3

**Example 2:**

**Input:** nums = [4,2,3,4]
**Output:** 4

**Constraints:**

-   `1 <= nums.length <= 1000`
-   `0 <= nums[i] <= 1000`


# Solution: O(n^2)
```
class Solution {
    public int triangleNumber(int[] nums) {
        if(nums.length < 3) return 0;
        int res = 0, n = nums.length;
        Arrays.sort(nums);
        
        for(int i = n - 1; i >= 2; i--){
            int start = 0, end = i - 1;
            while(start < end){
                if(nums[start] + nums[end] > nums[i]){
//                     fix nums[end]
                    res += (end - start);
                    end--;
                }else{
                    start++;
                }
            }
        }
        
        return res;
    }
}
```