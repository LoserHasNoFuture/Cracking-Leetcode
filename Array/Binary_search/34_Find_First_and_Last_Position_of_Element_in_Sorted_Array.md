# 34. Find First and Last Position of Element in Sorted Array
Given an array of integers  `nums`  sorted in ascending order, find the starting and ending position of a given  `target`  value.

If  `target`  is not found in the array, return  `[-1, -1]`.

You must write an algorithm with `O(log n)`  runtime complexity.

**Example 1:**

**Input:** nums = [5,7,7,8,8,10], target = 8
**Output:** [3,4]

**Example 2:**

**Input:** nums = [5,7,7,8,8,10], target = 6
**Output:** [-1,-1]

**Example 3:**

**Input:** nums = [], target = 0
**Output:** [-1,-1]

**Constraints:**

-   `0 <= nums.length <= 105`
-   `-109 <= nums[i] <= 109`
-   `nums`  is a non-decreasing array.
-   `-109 <= target <= 109`

# Solution 1: Binary Search (Beat 100%)
```
class Solution {
    public int[] searchRange(int[] nums, int target) {
        if(nums.length == 0) return new int[]{-1,-1};
        int first = find_first(nums,target);
        if(first == -1) return new int[]{-1,-1};
        int last = find_last(nums,target,first);
        return new int[]{first, last};
    }
    
    public int find_last(int[] nums, int target, int start){
        int end = nums.length - 1;
        
        while(start < end){
            int mid = (start + end + 1) >>> 1;
            if(nums[mid] == target) start = mid;
            else end = mid - 1;
        }
        
        return end;
    }
    
    public int find_first(int[] nums, int target){
        int start = 0, end = nums.length - 1;
        
        while(start < end){
            int mid = (start + end) >>> 1;
            if(nums[mid] >= target) end = mid;
            else start = mid + 1;
        }
        
        return nums[start] == target? start:-1;
    }
}
```


Considering target and elements in A are integers, we can implement only one binary search function. Refer from:https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/discuss/14701/A-very-simple-Java-solution-with-only-one-binary-search-algorithm

Drawbacks: target + 1 may lead to overflow.  (Luckily in this problem, the target is bound)


```
public class Solution {
    public int[] searchRange(int[] A, int target) {
        int start = Solution.firstGreaterEqual(A, target);
        if (start == A.length || A[start] != target) {
            return new int[]{-1, -1};
        }
        return new int[]{start, Solution.firstGreaterEqual(A, target + 1) - 1};
    }

    private static int firstGreaterEqual(int[] A, int target) {
        int low = 0, high = A.length;
        while (low < high) {
            int mid = low + ((high - low) >> 1);
            //low <= mid < high
            if (A[mid] < target) {
                low = mid + 1;
            } else {
                //should not be mid-1 when A[mid]==target.
                //could be mid even if A[mid]>target because mid<high.
                high = mid;
            }
        }
        return low;
    }
}
```