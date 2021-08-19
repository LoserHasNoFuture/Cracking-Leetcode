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
    public int[][] intervalIntersection(int[][] flist, int[][] slist) {
        int ptr1 = 0, ptr2 = 0, n = flist.length, m = slist.length;
        List<int[]> arr = new ArrayList<int[]>();
        
        while(ptr1 < n && ptr2 < m){
            if(flist[ptr1][0] > slist[ptr2][1]) ptr2++;
            else if(slist[ptr2][0] > flist[ptr1][1]) ptr1++;
            else{
                int start = Math.max(flist[ptr1][0],slist[ptr2][0]);
                int end = Math.min(flist[ptr1][1],slist[ptr2][1]);
                arr.add(new int[]{start,end});
                
                if(flist[ptr1][1] > end){
                    flist[ptr1][0] = end;
                }else ptr1++;
                
                
                if(slist[ptr2][1] > end){
                    slist[ptr2][0] = end;
                }else ptr2++;
            }
        }
        
        int[][] res= new int[arr.size()][2];
        int index = 0;
        for(int[] meet: arr){
            res[index++] = meet;
        }
        
        return res;
    }
}
```

Better Coding:
```
class Solution {
    public int[][] intervalIntersection(int[][] flist, int[][] slist) {
        int ptr1 = 0, ptr2 = 0, n = flist.length, m = slist.length;
        List<int[]> arr = new ArrayList<int[]>();
        
        while(ptr1 < n && ptr2 < m){
            int start = Math.max(flist[ptr1][0],slist[ptr2][0]);
            int end = Math.min(flist[ptr1][1],slist[ptr2][1]);
            if(start <= end) arr.add(new int[]{start,end});

            if(flist[ptr1][1] > end) ptr2++;
            else ptr1++;
        }
        
        int[][] res= new int[arr.size()][2];
        int index = 0;
        for(int[] meet: arr){
            res[index++] = meet;
        }
        
        return res;
    }
}
```

# Solution 2: 
if M << N or N << M, consider binary search.
O(M log N) , O(N log M)