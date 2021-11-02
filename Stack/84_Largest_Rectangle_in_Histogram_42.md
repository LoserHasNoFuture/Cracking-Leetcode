# 84. Largest Rectangle in Histogram 42
Given an array of integers  `heights`  representing the histogram's bar height where the width of each bar is  `1`, return  _the area of the largest rectangle in the histogram_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg)

**Input:** heights = [2,1,5,6,2,3]
**Output:** 10
**Explanation:** The above is a histogram where width of each bar is 1.
The largest rectangle is shown in the red area, which has an area = 10 units.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/04/histogram-1.jpg)

**Input:** heights = [2,4]
**Output:** 4

**Constraints:**

-   `1 <= heights.length <= 105`
-   `0 <= heights[i] <= 104`

# Solution: Using Stack (Beat 100%)
```
class Solution {
    // when the height is increasing, we don't have to calculate the area
    // because the area is also increasing
    // only when the height starts to decreas, we calculate the area
    public int largestRectangleArea(int[] height) {
        int res = 0, index = -1, n = height.length;
        int[] stack = new int[n];
        
        for(int i = 0; i < n; i++){
            int cur = height[i];
            while(index >= 0 && height[stack[index]] > cur){
                int hi = height[stack[index--]];
                int width = index >= 0? i - stack[index] - 1: i;
                res = Math.max(width * hi, res);
            }
            stack[++index] = i;
        }
        
        for(int i = index; i >= 0; i--){
            int width = i > 0? n - stack[i-1] - 1: n;
            res = Math.max(res, width*height[stack[i]]);
        }
        
        return res;
    }
}
```

# Solution 2: Using DP (Beat 100%)
Refer from: [https://leetcode.com/problems/largest-rectangle-in-histogram/discuss/28902/5ms-O(n)-Java-solution-explained-(beats-96)](https://leetcode.com/problems/largest-rectangle-in-histogram/discuss/28902/5ms-O(n)-Java-solution-explained-(beats-96))
```
class Solution {
    public static int largestRectangleArea(int[] height) {
        if (height == null || height.length == 0) {
            return 0;
        }
        int n = height.length, res= 0;
        int[] leftless = new int[n];
        int[] rightless = new int[n];
        
        for(int i = 0; i < n; i++){
            int left = i-1;
            while(left >= 0 && height[left] >= height[i]) left = leftless[left];
            leftless[i] = left;
        }
        
        for(int i = n-1; i >= 0; i--){
            int right = i+1;
            while(right < n && height[right] >= height[i]) right = rightless[right];
            rightless[i] = right;
        }
        
        for(int i = 0; i < n; i++){
            res = Math.max(res, (rightless[i]-leftless[i]-1)*height[i]);
        }

        return res;
    }
}
```