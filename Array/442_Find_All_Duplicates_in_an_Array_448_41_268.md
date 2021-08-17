# 442. Find All Duplicates in an Array  (Do 448 First)
Given an array of integers, 1 ≤ a[i] ≤  _n_  (_n_  = size of array), some elements appear  **twice**  and others appear  **once**.

Find all the elements that appear  **twice**  in this array.

Could you do it without extra space and in O(_n_) runtime?

**Example:**  

**Input:**
[4,3,2,7,8,2,3,1]

**Output:**
[2,3]

# Solution: 
Since:
1) each element either appears once or twice
2) and its value is in range [1,n].. 

So we can consider the value as index.
If one element appears, we change the index value to be negative....
If we find a value to be negative, which simple means the value has been appeared before.
```
class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        List<Integer> res = new LinkedList<Integer>();
        
        for(int i = 0; i < nums.length; i++){
            int index = Math.abs(nums[i]) - 1;
            if(nums[index] < 0) res.add(index + 1);
            else nums[index] = -nums[index];
        }
        
        return res;
    }
}
```

If one element appears more than two times, we can solve this problem by simply changing **res** to be set.   If res is set, we can also use cyclic sort to solve it.


# Solution: Cyclic Sort
```
class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        int n = nums.length;
        for(int i = 0; i < n; i++){
            while(nums[i] != i+1){
                int tmp = nums[i];
                if(nums[tmp-1] == tmp) {
                    break;
                }else swap(nums, i, tmp-1);
            }
        }
        List<Integer> res = new LinkedList<Integer>();
        
        for(int i = 0; i < n; i++){
            if(nums[i] != i+1) res.add(nums[i]);
        }
        
        return res; 
    }
    
    public void swap(int[] nums, int a, int b){
        int tmp = nums[a];
        nums[a] = nums[b];
        nums[b] = tmp;
    }
   
}
```