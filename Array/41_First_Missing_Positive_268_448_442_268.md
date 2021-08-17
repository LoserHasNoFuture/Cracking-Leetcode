# 41. First Missing Positive 268 448 442
Given an unsorted integer array, find the smallest missing positive integer.

**Example 1:**

Input: [1,2,0]
Output: 3

**Example 2:**

Input: [3,4,-1,1]
Output: 2

**Example 3:**

Input: [7,8,9,11,12]
Output: 1

**Follow up:**

Your algorithm should run in  _O_(_n_) time and uses constant extra space.

# Solution: Cyclic Sort
```
class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;
        for(int i = 0; i < n; i++){
            while(nums[i] > 0 && nums[i] <= n && nums[i] != i+1){
                int tmp = nums[i];
                if(nums[tmp-1] == tmp) break;
                swap(nums, tmp-1, i);
            }
        }
        
        for(int i = 0; i < n; i++){
            if(nums[i] != i+1) return i+1;
        }
        
        return n+1;
    }
    
    public void swap(int[] nums, int a, int b){
        int tmp = nums[a];
        nums[a] = nums[b];
        nums[b] = tmp;
    }
}
```

# Solution: Similar to 442
```
class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;
        for(int i = 0; i < n; i++){
            if(nums[i] <= 0) nums[i] = n+1;
        }
        
        for(int num: nums){
            int index = Math.abs(num) - 1;
            if(index < n && nums[index] > 0) nums[index] = -nums[index];
        }
        
        for(int i = 0; i < n; i++){
            if(nums[i] > 0) return i+1;
        }
        
        return n+1;
    }
    
}
```