# 31. Next Permutation
Implement  **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such an arrangement is not possible, it must rearrange it as the lowest possible order (i.e., sorted in ascending order).

The replacement must be  **[in place](http://en.wikipedia.org/wiki/In-place_algorithm)**  and use only constant extra memory.

**Example 1:**

**Input:** nums = [1,2,3]
**Output:** [1,3,2]

**Example 2:**

**Input:** nums = [3,2,1]
**Output:** [1,2,3]

**Example 3:**

**Input:** nums = [1,1,5]
**Output:** [1,5,1]

**Example 4:**

**Input:** nums = [1]
**Output:** [1]

**Constraints:**

-   `1 <= nums.length <= 100`
-   `0 <= nums[i] <= 100`

# Solution 1: My O(n^2) Solution
```
class Solution {
    
    public void nextPermutation(int[] nums) {
        int index = dfs(nums,0);
        sort(nums,index+1,nums.length-1);        
    }
    
    public int dfs(int[] nums, int index){        
        if(index >= nums.length-1) return -1;
        int res = dfs(nums,index + 1);
        if(res != -1) return res;
        
        for(int i = nums.length-1; i > index; i--){
            if(nums[i] > nums[index]){
                swap(nums,index,i);
                return index;
            }
        }
        return -1;
    }
    
    public void sort(int[] nums, int start, int end){
        if(start >= end) return;
        int l = start + 1, r = end, pivot = nums[start];
        
        while(l <= r){
            while(l <= r && nums[l] <= pivot) l++;
            while(l <= r && nums[r] >= pivot) r--;
            if(l <= r){
                swap(nums,l,r);
                l++;
                r--;
            }
        }
        swap(nums,r,start);
        sort(nums,r+1,end);
        sort(nums,start,r-1);
    }
    
    public void swap(int[] nums, int a, int b){
        int tmp = nums[a];
        nums[a] = nums[b];
        nums[b] = tmp;
    }
}
```

# Solution 2: Narayana Pandita's O(n) Solution
```
class Solution {
    public void nextPermutation(int[] A) {
        if(A == null || A.length <= 1) return;
        int i = A.length - 2;
        while(i >= 0 && A[i] >= A[i + 1]) i--; // Find 1st id i that breaks descending order
        if(i >= 0) {                           // If not entirely descending
            int j = A.length - 1;              // Start from the end
            while(A[j] <= A[i]) j--;           // Find rightmost first larger id j
            swap(A, i, j);                     // Switch i and j
        }
        reverse(A, i + 1, A.length - 1);       // Reverse the descending sequence
    }

    public void swap(int[] A, int i, int j) {
        int tmp = A[i];
        A[i] = A[j];
        A[j] = tmp;
    }

    public void reverse(int[] A, int i, int j) {
        while(i < j) swap(A, i++, j--);
    }
}
```