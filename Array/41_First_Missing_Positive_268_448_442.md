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
            
        for(int i = 0; i < nums.length; i++){
            while(nums[i] - 1 != i && nums[i] - 1 >= 0 && nums[i] - 1 < nums.length  && nums[nums[i] - 1] != nums[i]){
                swap(nums,nums[i]-1,i);
            }
        }
        
        for(int i = 0; i < nums.length; i++){
            if(nums[i]-1 != i) return i+1;
        }
        
        return nums.length + 1;
    }
    
    public void swap(int[] nums, int a, int b){
        int temp = nums[a];
        nums[a] = nums[b];
        nums[b] = temp;
    }
}
```

# Solution: Similar to 442
```
public class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            if (nums[i] <= 0 || nums[i] > n) {
                nums[i] = n + 1;
            }
        }

        for (int i = 0; i < n; i++) {
            int num = Math.abs(nums[i]);
            if (num > n) {
                continue;
            }
            num--;
            if (nums[num] > 0) {
                nums[num] = -1 * nums[num];
            }
        }

        for (int i = 0; i < n; i++) {
            if (nums[i] >= 0) {
                return i + 1;
            }
        }

        return n + 1;
    }
}
```