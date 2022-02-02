# 287. Find the Duplicate Number
Given an array of integers  `nums`  containing `n + 1`  integers where each integer is in the range  `[1, n]`  inclusive.

There is only  **one repeated number**  in  `nums`, return  _this repeated number_.

You must solve the problem  **without**  modifying the array  `nums` and uses only constant extra space.

**Example 1:**

**Input:** nums = [1,3,4,2,2]
**Output:** 2

**Example 2:**

**Input:** nums = [3,1,3,4,2]
**Output:** 3

**Constraints:**

-   `1 <= n <= 105`
-   `nums.length == n + 1`
-   `1 <= nums[i] <= n`
-   All the integers in  `nums`  appear only  **once**  except for  **precisely one integer**  which appears  **two or more**  times.

**Follow up:**

-   How can we prove that at least one duplicate number must exist in  `nums`?
-   Can you solve the problem in linear runtime complexity?


# Solution 1:
```
class Solution {
    public int findDuplicate(int[] nums) {      
        int start = 0;
        int end = nums.length - 1;
        while(start < end){
            int mid = (start + end + 1) >>> 1;
            int count = count_smaller_nums(nums,mid);
            if(count < mid) start = mid;
            else end = mid - 1;
        }
        
        return start;
    }
    
    public int count_smaller_nums(int[] nums,int mid){
        int count = 0;
        for(int i = 0; i < nums.length; i++){
            if(nums[i]< mid) count++;
        }
        return count;
    }
}
```

# Solution 2: LinkedList Cycle
```
class Solution {
    public int findDuplicate(int[] nums) {
        
        // 因为数字是从1-n, 而nums有n+1位, 所以index这样取没有问题
        int fast = nums[0], slow = nums[0];
        
        while(true){
            slow = nums[slow];
            fast = nums[nums[fast]];
            if(fast == slow) break;
        }
        
        fast = nums[0];
        while(slow != fast){
            slow = nums[slow];
            fast = nums[fast];
        }
        
        return slow;
    }
}
```
