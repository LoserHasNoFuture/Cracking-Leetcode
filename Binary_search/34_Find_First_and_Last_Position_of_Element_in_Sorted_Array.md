# 34. Find First and Last Position of Element in Sorted Array

Given an array of integers  `nums`  sorted in ascending order, find the starting and ending position of a given  `target`  value.

Your algorithm's runtime complexity must be in the order of  _O_(log  _n_).

If the target is not found in the array, return  `[-1, -1]`.

**Example 1:**

**Input:** nums = [`5,7,7,8,8,10]`, target = 8
**Output:** [3,4]

**Example 2:**

**Input:** nums = [`5,7,7,8,8,10]`, target = 6
**Output:** [-1,-1]

**Constraints:**

-   `0 <= nums.length <= 10^5`
-   `-10^9 <= nums[i] <= 10^9`
-   `nums`  is a non decreasing array.
-   `-10^9 <= target <= 10^9`

# Solution: Binary Search
```
class Solution {
    public int[] searchRange(int[] nums, int target) {
        if(nums.length == 0) return new int[]{-1,-1};
        int start = find_start(nums,target);
        if(start == -1) return new int[]{-1,-1};
        return new int[]{start,find_end(nums,target,start)};
    }
    
    public int find_end(int[] nums, int target, int start){
        int end = nums.length - 1;
        while(start < end){
            int mid = (start + end + 1) >>> 1;
            if(nums[mid] > target) end = mid - 1;
            else start = mid;
        }
        return start;
    }
    
    public int find_start(int[] nums, int target){
        int start = 0;
        int end = nums.length - 1;
        while(start < end){
            int mid = (start + end) >>> 1;
            if(nums[mid] < target) start = mid + 1;
            else end = mid;
        }
        
        if(nums[start] == target) return start;
        else return -1;
    }
}
```

Considering target and elements in A are integers, we can implement only one binary search function.
Refer from:[https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/discuss/14701/A-very-simple-Java-solution-with-only-one-binary-search-algorithm](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/discuss/14701/A-very-simple-Java-solution-with-only-one-binary-search-algorithm)

Drawbacks: target + 1 may lead to overflow.
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