# 56. Merge Intervals
Given an array of  `intervals` where  `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return  _an array of the non-overlapping intervals that cover all the intervals in the input_.

**Example 1:**

**Input:** intervals = [[1,3],[2,6],[8,10],[15,18]]
**Output:** [[1,6],[8,10],[15,18]]
**Explanation:** Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].

**Example 2:**

**Input:** intervals = [[1,4],[4,5]]
**Output:** [[1,5]]
**Explanation:** Intervals [1,4] and [4,5] are considered overlapping.

**Constraints:**

-   `1 <= intervals.length <= 104`
-   `intervals[i].length == 2`
-   `0 <= starti  <= endi  <= 104`

# Solution 1: Sort + Merge (Beat 94%)
```
class Solution {
    public int[][] merge(int[][] intervals) {
        ArrayList<int[]> res = new ArrayList<int[]>();
        Arrays.sort(intervals, (a,b)->(a[0]-b[0]));        
        int[] cur = intervals[0];
        for(int[] arr: intervals){
            if(arr[0] > cur[1]){
                res.add(cur);
                cur = arr;
            }else cur[1] = Math.max(cur[1],arr[1]);
        }
        res.add(cur);
        
        int[][] ret = new int[res.size()][2];
        
        for(int i = 0; i < res.size(); i++) ret[i] = res.get(i);
        
        return ret;
    }
}
```

# Solution 2: Sorting Start And End Respectively (Beat 99%)
Refer from: [https://leetcode.com/problems/merge-intervals/discuss/21223/Beat-98-Java.-Sort-start-and-end-respectively.](https://leetcode.com/problems/merge-intervals/discuss/21223/Beat-98-Java.-Sort-start-and-end-respectively.)
![](https://www.dropbox.com/s/40mmgsb710mqykt/Screenshot%202018-02-09%2018.43.49.png?raw=1)
```
class Solution {
    public int[][] merge(int[][] intervals) {
        int n = intervals.length;
        int[] start = new int[n];
        int[] end = new int[n];
        for(int i = 0; i < n; i++){
            start[i] = intervals[i][0];
            end[i] = intervals[i][1];
        }
        Arrays.sort(start);
        Arrays.sort(end);
        
        ArrayList<int[]> res = new ArrayList<int[]>();
        // using j to save the start of a new interval
        for(int i = 0, j = 0; i < n; i++){
            if(i == n - 1 || end[i] < start[i + 1] ){
                res.add(new int[]{start[j],end[i]});
                j = i + 1;
            }
        }
        
        int[][] ret = new int[res.size()][2];
        
        for(int i = 0; i < res.size(); i++) ret[i] = res.get(i);
        
        return ret;
    }
}
```

# Solution 3: FB Interview (Later)
Constraint: Not using any sort 

解法1: 遍历一遍找到最小的开始,最大的结束
建立一个boolean occupied = booean[max - min + 1], 然后遍历intervals填空...
最后遍历occupied出结果

解法2: [https://leetcode.com/problems/merge-intervals/discuss/21451/Share-my-BST-interval-tree-solution-C%2B%2B-No-sorting!](https://leetcode.com/problems/merge-intervals/discuss/21451/Share-my-BST-interval-tree-solution-C%2B%2B-No-sorting!)
