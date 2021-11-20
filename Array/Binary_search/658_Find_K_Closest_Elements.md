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
 
# Solution 1: Find x first then expand  (Beat 97%)
```
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        List<Integer> res = new ArrayList<Integer>();
        
        // the cloest index
        int left = find_target(arr,x) - 1;
        int right = left + 2;
        
        while(left >= 0 && right < arr.length && right - left - 1 < k){
            if(Math.abs(arr[left] - x) <= Math.abs(arr[right] - x)) left--;
            else right++;
        }
        
        if(left < 0){
            while(right < k) right++;
        }
        
        if(right >= arr.length){
            while(arr.length - left - 1 < k) left--;
        }
        
        for(int i = left + 1; i < right; i++) res.add(arr[i]);
        
        return res;
    }
    
    
    public int find_target(int[] arr, int target){
        int start = 0, end = arr.length - 1;
        while(start < end){
            int mid = (start + end + 1) >>> 1;
            if(arr[mid] == target) return mid;
            if(arr[mid] > target) end = mid - 1;
            else start = mid;
        }
        
        if(start == arr.length -1) return start;
        return target - arr[start] <= arr[start + 1] - target? start: start+ 1;
    }
    
    
}
```


# Solution 2: Using BS directly (Need more thinking, Beat 97%)
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