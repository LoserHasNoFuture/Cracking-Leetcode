# 4. Median of Two Sorted Arrays
Given two sorted arrays  `nums1`  and  `nums2`  of size  `m`  and  `n`  respectively, return  **the median**  of the two sorted arrays.

The overall run time complexity should be  `O(log (m+n))`.

**Example 1:**

**Input:** nums1 = [1,3], nums2 = [2]
**Output:** 2.00000
**Explanation:** merged array = [1,2,3] and median is 2.

**Example 2:**

**Input:** nums1 = [1,2], nums2 = [3,4]
**Output:** 2.50000
**Explanation:** merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.

**Example 3:**

**Input:** nums1 = [0,0], nums2 = [0,0]
**Output:** 0.00000

**Example 4:**

**Input:** nums1 = [], nums2 = [1]
**Output:** 1.00000

**Example 5:**

**Input:** nums1 = [2], nums2 = []
**Output:** 2.00000

**Constraints:**

-   `nums1.length == m`
-   `nums2.length == n`
-   `0 <= m <= 1000`
-   `0 <= n <= 1000`
-   `1 <= m + n <= 2000`
-   `-106  <= nums1[i], nums2[i] <= 106`


# Solution 1: O(m+n)
```
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length, mid = (m+n+1)/2;
        int ptr1 = 0, ptr2 = 0, res = 0;
        
        while(ptr1 < m || ptr2 < n){
            int val1 = ptr1 < m? nums1[ptr1]:Integer.MAX_VALUE;
            int val2 = ptr2 < n? nums2[ptr2]:Integer.MAX_VALUE;
            
            mid--;
            if(val1 < val2){
                ptr1++;
            }else{
                val1 = val2;
                ptr2++;
            }
            
            if(mid == 0){
                res += val1;
                if((m+n)%2 == 1) break;
            }
            
            if(mid == -1){
                res += val1;
                break;
            }
        }
        
        return (m+n)%2 == 1? (double) res :((double)res)/2;
    }
}
```

# Solution 2: O(log (min(m,n)))
Refer from: [https://leetcode.com/problems/median-of-two-sorted-arrays/discuss/2481/Share-my-O(log(min(mn)))-solution-with-explanation](https://leetcode.com/problems/median-of-two-sorted-arrays/discuss/2481/Share-my-O(log(min(mn)))-solution-with-explanation)
```
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        if(nums1.length > nums2.length){
            int[] tmp = nums1;
            nums1 = nums2;
            nums2 = tmp;
        }
        int n = nums1.length, m = nums2.length, start = 0, end = n;
        
        while(start < end){
            int i = (start + end) >>> 1, j = (m + n + 1)/2 - i;
            if((i == 0 || j == m || nums1[i-1] <= nums2[j]) &&
              (j == 0 || i == n || nums2[j-1] <= nums1[i])){
                start = i;
                break;
            }
            
            if(i > 0 && j < m && nums1[i-1] > nums2[j]) end = i;
            else start = i + 1;
        }
        
        int leftmax = 0, rightmin = 0;
        int i = start, j = (m + n + 1)/2 - i;
        if(i == 0 && j == 0) return 0.0;
        else if(i == 0) leftmax = nums2[j-1];
        else if(j == 0) leftmax = nums1[i-1];
        else leftmax = Math.max(nums1[i-1],nums2[j-1]);

        if((m+n)%2 == 1) return (double)leftmax; 


        if(i == n) rightmin = nums2[j];
        else if(j == m) rightmin = nums1[i];
        else rightmin = Math.min(nums2[j], nums1[i]);

        return ((double)(leftmax + rightmin))/2;
    }
}
```