# 852. Peak Index in a Mountain Array
Let's call an array  `A`  a  _mountain_ if the following properties hold:

-   `A.length >= 3`
-   There exists some  `0 < i < A.length - 1`  such that  `A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]`

Given an array that is definitely a mountain, return any `i` such that `A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]`.

**Example 1:**

**Input:** [0,1,0]
**Output:** 1

**Example 2:**

**Input:** [0,2,1,0]
**Output:** 1

**Note:**

1.  `3 <= A.length <= 10000`
2.  `0 <= A[i] <= 10^6`
3.  A is a mountain, as defined above.

# Solution 1: Brute Force
```
class Solution {
    public int peakIndexInMountainArray(int[] A) {
        if(A.length == 1) return A[0];
        
        int result = 0;
        int start = 0;
        int end = A.length - 1;
        
        while(start < end){
            if(A[start] < A[start+1]) start++;
            if(A[end] < A[end-1]) end--;
        }
        
        return start;
    }
}
```

# Solution 2: Binary Search
```
class Solution {
    public int peakIndexInMountainArray(int[] A) {
        if(A.length == 1) return A[0];
        
        int result = 0;
        int start = 0;
        int end = A.length - 1;
        
        while(start < end){
            int mid = (start + end)/2;
            if(A[mid] < A[mid+1]) start = mid+1;
            else end = mid;
        }
        
        return start;
        
    }
}
```