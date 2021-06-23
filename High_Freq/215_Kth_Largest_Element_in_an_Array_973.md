# 215. Kth Largest Element in an Array  973
Given an integer array  `nums`  and an integer  `k`, return  _the_  `kth`  _largest element in the array_.

Note that it is the  `kth`  largest element in the sorted order, not the  `kth`  distinct element.

**Example 1:**

**Input:** nums = [3,2,1,5,6,4], k = 2
**Output:** 5

**Example 2:**

**Input:** nums = [3,2,3,1,2,4,5,5,6], k = 4
**Output:** 4

**Constraints:**

-   `1 <= k <= nums.length <= 104`
-   `-104  <= nums[i] <= 104`

# Solution 1: Quick Selection (Beat 100%)
```
class Solution {
    public int findKthLargest(int[] nums, int k) {
        return quick_selection(nums,0,nums.length - 1,nums.length - k);
    }
    
    public int quick_selection(int[] nums,int start,int end,int k){
        if(start == end) return nums[start];
        if(start > end) return -1;
        int l = start+1, r = end;
        int pivot = nums[start];
        
        while(l <= r){
            while(l <= r && nums[l] <= pivot) l++;
            while(l <= r && nums[r] >= pivot) r--;
            
            if(l <= r) swap(nums,l,r);
            
        }
        
        swap(nums,r,start);
        if(r == k) return nums[r];
        if(r < k) return quick_selection(nums,r+1,end,k);
        else return quick_selection(nums,start,r-1,k);
    }
    
    public void swap(int[] nums, int a, int b){
        int tmp = nums[a];
        nums[a] = nums[b];
        nums[b] = tmp;
    }
}
```