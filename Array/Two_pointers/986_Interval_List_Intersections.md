# 986. Interval List Intersections
Given two lists of  **closed**  intervals, each list of intervals is pairwise disjoint and in sorted order.

Return the intersection of these two interval lists.

_(Formally, a closed interval  `[a, b]`  (with  `a <= b`) denotes the set of real numbers  `x`  with  `a <= x <= b`. The intersection of two closed intervals is a set of real numbers that is either empty, or can be represented as a closed interval. For example, the intersection of [1, 3] and [2, 4] is [2, 3].)_

**Example 1:**

**![](https://assets.leetcode.com/uploads/2019/01/30/interval1.png)**

**Input:** A = [[0,2],[5,10],[13,23],[24,25]], B = [[1,5],[8,12],[15,24],[25,26]]
**Output:** [[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]

**Note:**

1.  `0 <= A.length < 1000`
2.  `0 <= B.length < 1000`
3.  `0 <= A[i].start, A[i].end, B[i].start, B[i].end < 10^9`

# Solution 1: Straightforward, O(m+n)  (Beat 60%)
```
class Solution {
    public int[][] intervalIntersection(int[][] A, int[][] B) {
        int lenA  = A.length, lenB = B.length;
        
        ArrayList<int[]> res = new ArrayList<int[]>();
        for(int i = 0, j = 0; i < lenA && j < lenB; ){
            int start = Math.max(A[i][0], B[j][0]);
            int end = Math.min(A[i][1], B[j][1]);
            if(start <= end) res.add(new int[]{start,end});
            
            if(A[i][1] < B[j][1]) {
                B[j][0] = start;
                i++;    
            }else{
                A[i][0] = start;
                j++;
            }
        }
        
        int[][] ret = new int[res.size()][2];
        for(int i = 0; i < res.size(); i++) ret[i] = res.get(i);
        return ret;
    }
}
```

# Solution 2: 
if M << N or N << M, consider binary search.
O(M log N) , O(N log M)