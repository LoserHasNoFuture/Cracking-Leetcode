# 209. Minimum Size Subarray Sum
Given an array of  **n**  positive integers and a positive integer  **s**, find the minimal length of a  **contiguous**  subarray of which the sum ≥  **s**. If there isn't one, return 0 instead.

**Example:**

**Input:** `s = 7, nums = [2,3,1,2,4,3]`
**Output:** 2
**Explanation:** the subarray `[4,3]` has the minimal length under the problem constraint.

**Follow up:**

If you have figured out the  _O_(_n_) solution, try coding another solution of which the time complexity is  _O_(_n_  log  _n_).

# Solution 1: Two Pointers O(n)
```
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        if(nums.length == 0) return 0;
        int res = nums.length;
        
        int start = 0, end = 0, sum = 0;
        while(end < nums.length && start <= end){
            sum += nums[end];
            if(sum >= s) {
                res = Math.min(res, end-start+1);
                if(res == 1) return res;
                sum -= nums[start];
                sum -= nums[end];
                start++;
            }else end++;
        }
        
        if(end - start == nums.length && sum < s) return 0;
        else return res;
    }
}
```


# Solution 2: Binary Search
```
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        if(nums.length == 0) return 0;
        int len = nums.length;
        int[] sum = new int[len+1];
        int res = Integer.MAX_VALUE;
        for(int i = 1; i < len+1; i++) sum[i] = sum[i-1]+nums[i-1];
        
        for(int i = 0; i <= len; i++){
            int end = binary_search(sum,i,sum[i]+target);
            if(end < len+1) res = Math.min(res, end-i);
        }
        
        if(res == Integer.MAX_VALUE) return 0;
        return res;
    }
    
    public int binary_search(int[] sum, int i, int target){
        int start = i;
        int end = sum.length;
        
        while(start < end){
            int mid = (start + end) >>> 1;
            if(sum[mid] < target) start = mid + 1;
            else end = mid;
        }
        
        return start;
    }
    
}
```