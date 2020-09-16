# 268. Missing Number 448 41
Given an array containing  _n_  distinct numbers taken from  `0, 1, 2, ..., n`, find the one that is missing from the array.

**Example 1:**

**Input:** [3,0,1]
**Output:** 2

**Example 2:**

**Input:** [9,6,4,2,3,5,7,0,1]
**Output:** 8

**Note**:  
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

# Solution 1: Cyclic Sort
```
class Solution {
    public int missingNumber(int[] nums) {
        for(int i = 0; i < nums.length; i++){
            while(nums[i] != i && nums[i] != nums.length){
                swap(nums, nums[i], i);
            }
        }
        
        for(int i = 0; i < nums.length; i++){
            if(nums[i] == nums.length) return i;
        }
        return nums.length;
    }
    
    public void swap(int[] nums, int a, int b){
        int temp = nums[a];
        nums[a] = nums[b];
        nums[b] = temp;
    }
}
```

# Solution 2: Bit Manipulation (XOR)
```
class Solution {
//     because a^b^b = a 
    public int missingNumber(int[] nums) {
        int res = 0;
        for (int i = 0; i < nums.length; i++) {
            res = res ^ i ^ nums[i];
        }
        return res ^ nums.length;
    }
}
```

# Solution 3: Sum
```
class Solution {
    public int missingNumber(int[] nums) { //sum
        int len = nums.length;
        int sum = (0+len)*(len+1)/2;
        for(int i=0; i<len; i++)
            sum-=nums[i];
        return sum;
    }
}
```