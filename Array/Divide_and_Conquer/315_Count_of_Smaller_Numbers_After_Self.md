# 315. Count of Smaller Numbers After Self
You are given an integer array  `nums`  and you have to return a new  `counts`  array. The  `counts`  array has the property where  `counts[i]`  is the number of smaller elements to the right of  `nums[i]`.

**Example 1:**

**Input:** nums = [5,2,6,1]
**Output:** [2,1,1,0]
**Explanation:**
To the right of 5 there are **2** smaller elements (2 and 1).
To the right of 2 there is only **1** smaller element (1).
To the right of 6 there is **1** smaller element (1).
To the right of 1 there is **0** smaller element.

**Example 2:**

**Input:** nums = [-1]
**Output:** [0]

**Example 3:**

**Input:** nums = [-1,-1]
**Output:** [0,0]

**Constraints:**

-   `1 <= nums.length <= 105`
-   `-104  <= nums[i] <= 104`


# Solution 1: Merge Sort
```
class Solution {
    
    public List<Integer> countSmaller(int[] nums) {
        List<Integer> res = new ArrayList<Integer>();
        if(nums.length == 0) return res;
        
        int[] cnt = new int[nums.length];
        int[] index = new int[nums.length];
        
        for(int i = 0;i < nums.length; i++) index[i] = i;
        
        mergeSort(nums,index, cnt, 0, nums.length - 1);
        
        
        for(int num: cnt) res.add(num);
        
        return res;
    }
    
    public void mergeSort(int[] nums, int[] index, int[] cnt, int start, int end){
        if(start >= end) return;
        
        int mid = (start + end) >>> 1;
        mergeSort(nums,index, cnt, start, mid);
        mergeSort(nums,index, cnt, mid+1, end);
        
        merge(nums,index, cnt, start, end);
    }
    
    public void merge(int[] nums, int[] index, int[] cnt, int start, int end){
        int mid = (start + end) >>> 1;
        int left = start, right = mid + 1;
        int ptr = 0, rightcount = 0;
        
        int[] new_index = new int[end-start+1];
        while(left <= mid && right <= end){
            if(nums[index[left]] <= nums[index[right]]){
                new_index[ptr++] = index[left];
                cnt[index[left]] += rightcount;
                left++;
            }else{
                new_index[ptr++] = index[right];
                rightcount++;
                right++;
            }
        }
        
        while(left <= mid){
            new_index[ptr++] = index[left];
            cnt[index[left]] += rightcount;
            left++;
        }
        
        while(right <= end){
            new_index[ptr++] = index[right];
            right++;
        }
        
        for(int i = start; i <= end; i++) index[i] = new_index[i-start];
        
    }
}
```