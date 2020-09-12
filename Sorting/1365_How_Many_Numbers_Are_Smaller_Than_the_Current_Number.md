# 1365. How Many Numbers Are Smaller Than the Current Number

Given the array  `nums`, for each  `nums[i]`  find out how many numbers in the array are smaller than it. That is, for each  `nums[i]`  you have to count the number of valid  `j's` such that `j != i`  **and**  `nums[j] < nums[i]`.

Return the answer in an array.

**Example 1:**

**Input:** nums = [8,1,2,2,3]
**Output:** [4,0,1,1,3]
**Explanation:** 
For nums[0]=8 there exist four smaller numbers than it (1, 2, 2 and 3). 
For nums[1]=1 does not exist any smaller number than it.
For nums[2]=2 there exist one smaller number than it (1). 
For nums[3]=2 there exist one smaller number than it (1). 
For nums[4]=3 there exist three smaller numbers than it (1, 2 and 2).

**Example 2:**

**Input:** nums = [6,5,4,8]
**Output:** [2,1,0,3]

**Example 3:**

**Input:** nums = [7,7,7,7]
**Output:** [0,0,0,0]

**Constraints:**

-   `2 <= nums.length <= 500`
-   `0 <= nums[i] <= 100`

# Solution 1: Using Comparison-based Sorting O(nlogn)
```
class Solution {
    public int[] smallerNumbersThanCurrent(int[] nums) {
        if(nums.length == 0) return nums;
        if(nums.length == 1) return new int[] {0};
        int len = nums.length;
        int[] score = new int[len];
        int[] res = new int[len];
        
        for(int i = 0; i < len; i++) score[i] = nums[i]*len + i;
        Arrays.sort(score);
        
        res[score[0]%len] = 0; 
        int prev = score[0]/len;
        for(int i = 1; i < len; i++){
            if(score[i]/len == prev) res[score[i]%len] = res[score[i-1]%len];
            else{
                res[score[i]%len] = i;
                prev = score[i]/len;
            }
        }
        
        return res;
    }
}
```

# Solution 2: Bucket Sort -->(If numbers go larger, can use radix sort)
```
class Solution {
    public int[] smallerNumbersThanCurrent(int[] nums) {
        if(nums.length == 0) return nums;
        if(nums.length == 1) return new int[] {0};
        int len = nums.length;
       
        int[] bucket = new int[101];
        for(int i = 0; i < len; i++) bucket[nums[i]]++;

        int[] sum = new int[101];
        for(int i = 1; i < 101; i++) sum[i] = bucket[i-1] + sum[i-1];
        
        int[] res = new int[len];
        for(int i = 0; i < len; i++) res[i] = sum[nums[i]];
        
        return res;
    }
}
```
Others' idea, using just one extra array:
```
class Solution {
    public int[] smallerNumbersThanCurrent(int[] nums) {
        if(nums.length == 0) return nums;
        if(nums.length == 1) return new int[] {0};
        int len = nums.length;
       
        int[] bucket = new int[101];
        for(int i = 0; i < len; i++) bucket[nums[i]]++;
        for(int i = 1; i < 101; i++) bucket[i] += bucket[i-1];
        
        int[] res = new int[len];
        for(int i = 0; i < len; i++) {
            if(nums[i] == 0) res[i] = 0; 
            else res[i] = bucket[nums[i]-1];
        }
        
        return res;
    }
}
```