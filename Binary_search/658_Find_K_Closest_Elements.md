# 658. Find K Closest Elements
Given a sorted array  `arr`, two integers  `k`  and  `x`, find the  `k`  closest elements to  `x`  in the array. The result should also be sorted in ascending order. If there is a tie, the smaller elements are always preferred.

**Example 1:**

**Input:** arr = [1,2,3,4,5], k = 4, x = 3
**Output:** [1,2,3,4]

**Example 2:**

**Input:** arr = [1,2,3,4,5], k = 4, x = -1
**Output:** [1,2,3,4]

**Constraints:**

-   `1 <= k <= arr.length`
-   `1 <= arr.length <= 10^4`
-   Absolute value of elements in the array and  `x`  will not exceed 104

# Solution 1: Find x first then expand
```
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        List<Integer> list = new ArrayList<Integer>();
        if(k == 0) return list;
        
        int start = find_closest_pos(arr,x);
        int end = start+1;
        
        while(end < arr.length && start >= 0 && k > 0){
            if(arr[end] - x >= x - arr[start]) {
                list.add(0,arr[start]);
                start--;
            }else{
                list.add(arr[end]);
                end++;
            }
            k--;
        }
        
        while(k > 0 && end < arr.length){
            list.add(arr[end]);
            k--;end++;
        }
        
        while(k > 0 && start >= 0){
            list.add(0,arr[start]);
            k--;start--;
        }
        return list;
    }
    
    public int find_closest_pos(int[] nums, int target){
        int start = 0;
        int end = nums.length;
        
        while(start < end){
            int mid = (start + end) >>> 1;
            if(nums[mid] < target) start = mid + 1;
            else end = mid;
        }
        
        if(start == nums.length) return nums.length-1;
        if(start == 0) return 0;
        return nums[start] - target >= target - nums[start-1]? start-1:start;
    }
}
```


# Solution 2: Using BS directly
```
class Solution {
    public List<Integer> findClosestElements(int[] A, int k, int x) {
        int left = 0, right = A.length - k;
        while (left < right) {
            int mid = (left + right) / 2;
            if (x - A[mid] > A[mid + k] - x)
                left = mid + 1;
            else
                right = mid;
        }
        return Arrays.stream(A, left, left + k).boxed().collect(Collectors.toList());
    }
}
```