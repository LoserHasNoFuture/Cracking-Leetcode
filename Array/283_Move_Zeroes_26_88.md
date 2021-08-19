# 283. Move Zeroes 
Given an array  `nums`, write a function to move all  `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Example:**

**Input:** `[0,1,0,3,12]`
**Output:** `[1,3,12,0,0]`

**Note**:

1.  You must do this  **in-place**  without making a copy of the array.
2.  Minimize the total number of operations.

# Solution
```
class Solution {
    public void moveZeroes(int[] nums) {
        if(nums.length <= 1) return;
        int move = 0;
        
        for(int i = 0; i < nums.length; i++){
            nums[i-move] = nums[i];
            if(nums[i] == 0) move++;
            if(move > 0) nums[i] = 0;
        }
        
        return;
    }
}
```