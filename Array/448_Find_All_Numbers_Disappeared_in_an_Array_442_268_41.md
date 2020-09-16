# 448. Find All Numbers Disappeared in an Array 442 268 41
Given an array of integers where 1 ≤ a[i] ≤  _n_  (_n_  = size of array), some elements appear twice and others appear once.

Find all the elements of [1,  _n_] inclusive that do not appear in this array.

Could you do it without extra space and in O(_n_) runtime? You may assume the returned list does not count as extra space.

**Example:**

**Input:**
[4,3,2,7,8,2,3,1]

**Output:**
[5,6]

# Solution: Cyclic Sort
```
class Solution {
//     Cyclic Sort
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> res = new ArrayList<Integer>();
        
        for(int i = 0; i < nums.length; i++){
            while(nums[i] - 1 != i){
                int index = nums[i] - 1;
                if(nums[index] == index + 1) break;
                else swap(nums, index, i);
            }
        }
        
        for(int i = 0; i < nums.length; i++){
            if(nums[i] - 1 != i) res.add(i+1);
        }
        
        return res;
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
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> res = new ArrayList<Integer>();
        
        for(int i = 0; i < nums.length; i++){
            int index = Math.abs(nums[i]) - 1;
            if(nums[index] > 0) nums[index] = -nums[index];
        }
        
        for(int i = 0; i < nums.length; i++){
            if(nums[i] > 0) res.add(i+1);
        }
        
        return res;
    }
    
}
```

Better Coding Skill:  (if n is super large, may overflow)
```
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> res = new ArrayList<>();
        int n = nums.length;
        for (int i = 0; i < nums.length; i ++) nums[(nums[i]-1) % n] += n;
        for (int i = 0; i < nums.length; i ++) if (nums[i] <= n) res.add(i+1);
        return res;
    } 
}
```
