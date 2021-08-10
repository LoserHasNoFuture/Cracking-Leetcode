# 42. Trapping Rain Water
Given  `n`  non-negative integers representing an elevation map where the width of each bar is  `1`, compute how much water it can trap after raining.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

**Input:** height = [0,1,0,2,1,0,1,3,2,1,2,1]
**Output:** 6
**Explanation:** The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.

**Example 2:**

**Input:** height = [4,2,0,3,2,5]
**Output:** 9

**Constraints:**

-   `n == height.length`
-   `0 <= n <= 3 * 104`
-   `0 <= height[i] <= 105`


# Solution 1: O(n) Time, O(n) Space
```
class Solution {
    public int trap(int[] height) {
        int n = height.length, res = 0;
        int[] heighestleft = new int[n], highestright = new int[n];
        
        for(int i = 0; i < n; i++){
            if(i == 0) heighestleft[i] = height[i];
            else heighestleft[i] = Math.max(height[i], heighestleft[i-1]);
        }
        
        for(int i = n - 1; i >= 0; i--){
            if(i == n-1) highestright[i] = height[i];
            else highestright[i] = Math.max(height[i], highestright[i+1]);
            
            res += Math.min(highestright[i], heighestleft[i]) - height[i];
        }
        
        return res;
    }
}
```

# Solution 2: Two pointers, O(n) Time, O(1) Space
```
class Solution {
    public int trap(int[] height) {
        int lh = 0, rh = 0, res = 0;
        int left = 0, right = height.length - 1;
        
        while(left <= right){
            lh = Math.max(lh, height[left]);
            rh = Math.max(rh, height[right]);
            
            if(lh < rh){
                res += lh - height[left];
                left++;
            }else {
                res += rh - height[right];
                right--;
            }
        }
        
        return res;
    }
}
```