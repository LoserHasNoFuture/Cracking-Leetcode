# 452. Minimum Number of Arrows to Burst Balloons 435
There are some spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. Since it's horizontal, y-coordinates don't matter, and hence the x-coordinates of start and end of the diameter suffice. The start is always smaller than the end.

An arrow can be shot up exactly vertically from different points along the x-axis. A balloon with  `xstart`  and  `xend`  bursts by an arrow shot at  `x`  if  `xstart  ≤ x ≤ xend`. There is no limit to the number of arrows that can be shot. An arrow once shot keeps traveling up infinitely.

Given an array  `points`  where  `points[i] = [xstart, xend]`, return  _the minimum number of arrows that must be shot to burst all balloons_.

**Example 1:**

**Input:** points = [[10,16],[2,8],[1,6],[7,12]]
**Output:** 2
**Explanation:** One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) and another arrow at x = 11 (bursting the other two balloons).

**Example 2:**

**Input:** points = [[1,2],[3,4],[5,6],[7,8]]
**Output:** 4

**Example 3:**

**Input:** points = [[1,2],[2,3],[3,4],[4,5]]
**Output:** 2

**Constraints:**

-   `1 <= points.length <= 104`
-   `points[i].length == 2`
-   `-231  <= xstart  < xend  <= 231  - 1`


# Solution 1: Sort the end
Refer: [https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/discuss/887875/If-you-cannot-pass-2147483646-214748364521474836462147483647](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/discuss/887875/If-you-cannot-pass-2147483646-214748364521474836462147483647)
**Apparently a new test case has been added recently. If you cannot pass this one, then it is because the result of subtraction is too large and thus the overflow is encountered. So don't use `a-b` to compare when sorting. Use `Integer.compare(a,b)` instead!!!**
```
class Solution {
    public int findMinArrowShots(int[][] points) {
        if(points.length == 0) return 0;
        // The compare part is very important!!!
        // Consider this case: [[-2147483646,-2147483645],[2147483646,2147483647]]
        Arrays.sort(points,(a,b)->Integer.compare(a[1],b[1]));
        int cnt = 1, arrow = points[0][1], n = points.length, ptr = 1;
        
        // System.out.println(arrow);
        while(ptr < n){
            if(points[ptr][0] > arrow) {
                arrow = points[ptr][1];
                cnt++;
            }
            
            ptr++;
        }
        
        return cnt;
    }
}
```

# Solution 2: Sort the start
```
class Solution {
    public int findMinArrowShots(int[][] points) {
        if(points.length == 0) return 0;
        Arrays.sort(points,(a,b)->Integer.compare(a[0],b[0]));
        int res = 1, end = points[0][1];
        
        for(int i = 1; i < points.length; i++){
            if(points[i][0] > end) {
                res++;
                end = points[i][1];
            }else end = Math.min(end, points[i][1]);
        }
        
        return res;
    }
}
```