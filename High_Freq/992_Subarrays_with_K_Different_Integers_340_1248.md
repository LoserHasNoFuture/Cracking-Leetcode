# 992. Subarrays with K Different Integers 340 1288
Given an integer array  `nums`  and an integer  `k`, return  _the number of  **good subarrays**  of_ `nums`.

A  **good array**  is an array where the number of different integers in that array is exactly  `k`.

-   For example,  `[1,2,3,1,2]`  has  `3`  different integers:  `1`,  `2`, and  `3`.

A  **subarray**  is a  **contiguous**  part of an array.

**Example 1:**

**Input:** nums = [1,2,1,2,3], k = 2
**Output:** 7
**Explanation:** Subarrays formed with exactly 2 different integers: [1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2]

**Example 2:**

**Input:** nums = [1,2,1,3,4], k = 3
**Output:** 3
**Explanation:** Subarrays formed with exactly 3 different integers: [1,2,1,3], [2,1,3], [1,3,4].

**Constraints:**

-   `1 <= nums.length <= 2 * 104`
-   `1 <= nums[i], k <= nums.length`

# Solution: 
思路比较刁钻， 是一个套娃题， 原始题为340。  
主要思路就是求最多有k个distinct character
然后用at_most(k) - at_most(k-1) = exact(k)
```
class Solution {
    public int subarraysWithKDistinct(int[] nums, int k) {
        return at_most(k, nums) - at_most(k-1, nums);
    }
    
    public int at_most(int k, int[] nums){
        if(k == 0) return 0;
        int n = nums.length, left = 0, right = 0, res = 0;
        HashMap<Integer,Integer> cnt = new HashMap<Integer,Integer>();
        
        while(right < n){
            int num = nums[right];
            cnt.put(num, cnt.getOrDefault(num,0) + 1);
            
            while(cnt.size() > k){
                int left_cnt = cnt.get(nums[left]);
                if(left_cnt == 1) cnt.remove(nums[left]);
                else cnt.put(nums[left],left_cnt-1);
                left++;
            }
            
            // number of subarrays end at right
            res += right - left + 1; 
            right++;
        }
        
        return res;
    }
}
```