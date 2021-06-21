# 33. Search in Rotated Sorted Array 81 153 154
Given an integer array  `nums`  sorted in ascending order, and an integer  `target`.

Suppose that  `nums`  is rotated at some pivot unknown to you beforehand (i.e.,  `[0,1,2,4,5,6,7]`  might become  `[4,5,6,7,0,1,2]`).

You should search for `target`  in  `nums`  and if you found return its index, otherwise return  `-1`.

**Example 1:**

**Input:** nums = [4,5,6,7,0,1,2], target = 0
**Output:** 4

**Example 2:**

**Input:** nums = [4,5,6,7,0,1,2], target = 3
**Output:** -1

**Example 3:**

**Input:** nums = [1], target = 0
**Output:** -1

**Constraints:**

-   `1 <= nums.length <= 5000`
-   `-10^4 <= nums[i] <= 10^4`
-   All values of  `nums`  are  **unique**.
-   `nums`  is guranteed to be rotated at some pivot.
-   `-10^4 <= target <= 10^4`

# Solution: Binary Search
The position of pivot can be decided by the order of **nums[start]**, **nums[mid]**, **nums[end]**.
```
class Solution {
    public int search(int[] arr, int target) {
        int start = 0;
        int end = arr.length - 1;

        while(start < end){
            int mid = (start + end) >>> 1;
            if(arr[mid] == target) return mid;
            if(arr[mid] > arr[end]){
//                 minimum at right
                if(arr[mid] >= target && arr[start] <= target){
                    end = mid -1;
                }else{
                    start = mid + 1;
                }
            }else if (arr[mid] <= arr[start]){
//                 minimum at the mid or the left
                if(arr[mid] < target && arr[end] >= target){
                    start = mid + 1;
                }else{
                    end = mid - 1;
                }
            }else{
//                 in order
                if(arr[mid] >= target) end = mid;
                else start = mid + 1;
            }
        }       

        return arr[start] == target? start: -1;
    }
}
```