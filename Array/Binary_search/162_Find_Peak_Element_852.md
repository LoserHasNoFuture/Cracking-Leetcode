# 162. Find Peak Element 852
A peak element is an element that is greater than its neighbors.

Given an input array  `nums`, where  `nums[i] ≠ nums[i+1]`, find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that  `nums[-1] = nums[n] = -∞`.

**Example 1:**

**Input:** **nums** = `[1,2,3,1]`
**Output:** 2
**Explanation:** 3 is a peak element and your function should return the index number 2.

**Example 2:**

**Input:** **nums** = `[`1,2,1,3,5,6,4]
**Output:** 1 or 5 
**Explanation:** Your function can return either index number 1 where the peak element is 2, 
             or index number 5 where the peak element is 6.

**Follow up:** Your solution should be in logarithmic complexity.

# Solution: Binary Search 1 (Lame)
```
class Solution {
    public int findPeakElement(int[] nums) {
        int start = 0;
        int end = nums.length - 1;
        
        while(start < end){
            int mid = (start + end) >>> 1;
            if(nums[mid] <= nums[end]) start = mid + 1;
            else if(nums[mid] <= nums[start]) end = mid - 1;
            else{
                start++;
                end--;
            }
        }
        
        return start;
    }
}
```

# Solution: Binary Search 2
Refer to: [https://leetcode.com/problems/find-peak-element/discuss/50239/Java-solution-and-explanation-using-invariants](https://leetcode.com/problems/find-peak-element/discuss/50239/Java-solution-and-explanation-using-invariants)
```
public int findPeakElement(int[] nums) {
    int N = nums.length;
    if (N == 1) {
        return 0;
    }
   
    int left = 0, right = N - 1;
    while (right - left > 1) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < nums[mid + 1]) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    return (left == N - 1 || nums[left] > nums[left + 1]) ? left : right;
}
```

# Solution: Binary Search 3
Refer from: [https://leetcode.com/problems/find-peak-element/discuss/50236/O(logN)-Solution-JavaCode](https://leetcode.com/problems/find-peak-element/discuss/50236/O(logN)-Solution-JavaCode)
```
public int findPeakElement(int[] num) {    
    return helper(num,0,num.length-1);
}

public int helper(int[] num,int start,int end){
    if(start == end){
        return start;
    }else if(start+1 == end){
        if(num[start] > num[end]) return start;
        return end;
    }else{
        
        int m = (start+end)/2;
        
        if(num[m] > num[m-1] && num[m] > num[m+1]){

            return m;

        }else if(num[m-1] > num[m] && num[m] > num[m+1]){

            return helper(num,start,m-1);

        }else{

            return helper(num,m+1,end);

        }
        
    }
}
```