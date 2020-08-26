# 287. Find the Duplicate Number (Similar to 142)
Given an array  _nums_  containing  _n_  + 1 integers where each integer is between 1 and  _n_  (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

**Example 1:**

**Input:** `[1,3,4,2,2]`
**Output:** 2

**Example 2:**

**Input:** [3,1,3,4,2]
**Output:** 3

**Note:**

1.  You  **must not**  modify the array (assume the array is read only).
2.  You must use only constant,  _O_(1) extra space.
3.  Your runtime complexity should be less than  _O_(_n_2).
4.  There is only one duplicate number in the array, but it could be repeated more than once.

# Solution 1: Binary Search
Given a number **mid**, if the number of elements smaller than **mid** is no less than **mid**, the duplicated number should be smaller than **mid**. 
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

# Solution: Slow and Fast Pointers
Refer from: [https://leetcode.com/problems/find-the-duplicate-number/discuss/72846/My-easy-understood-solution-with-O(n)-time-and-O(1)-space-without-modifying-the-array.-With-clear-explanation.](https://leetcode.com/problems/find-the-duplicate-number/discuss/72846/My-easy-understood-solution-with-O(n)-time-and-O(1)-space-without-modifying-the-array.-With-clear-explanation.)

Consider this array as a linked list:
    index is its value
    value is the value of next node
For example:

Given an array: 
|Index| 0| 1| 2| 3| 4| 
|-----|--|--|--|--|--|
|Value| 1| 3| 4| 2| 2|
We can consider it as the following linked list:

0->1->3->2->4---(back to)-->2

Because each input array has only one repeated number, which means the corresponding linked list forms one loop.  Then this problem becomes very similar to [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/), we need to find the start point of loop.
```
class Solution {
    public int findDuplicate(int[] nums) {      
        if(nums.length <= 1) return -1;
        int slow = nums[0];
        int fast = nums[0];
        
        while(true){
            slow = nums[slow];
            fast = nums[nums[fast]];
            if(slow == fast) break;
        }
        
        slow = nums[0];
        while(slow != fast){
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    } 
}
```
