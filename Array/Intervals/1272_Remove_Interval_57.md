# 1272. Remove Interval 57
A set of real numbers can be represented as the union of several disjoint intervals, where each interval is in the form  `[a, b)`. A real number  `x`  is in the set if one of its intervals  `[a, b)`  contains  `x`  (i.e.  `a <= x < b`).

You are given a  **sorted**  list of disjoint intervals  `intervals`  representing a set of real numbers as described above, where  `intervals[i] = [ai, bi]`  represents the interval  `[ai, bi)`. You are also given another interval  `toBeRemoved`.

Return  _the set of real numbers with the interval_ `toBeRemoved` _**removed**  from_  `intervals`_. In other words, return the set of real numbers such that every_ `x` _in the set is in_ `intervals` _but  **not**  in_ `toBeRemoved`_. Your answer should be a  **sorted**  list of disjoint intervals as described above._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/24/removeintervalex1.png)

**Input:** intervals = [[0,2],[3,4],[5,7]], toBeRemoved = [1,6]
**Output:** [[0,1],[6,7]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/24/removeintervalex2.png)

**Input:** intervals = [[0,5]], toBeRemoved = [2,3]
**Output:** [[0,2],[3,5]]

**Example 3:**

**Input:** intervals = [[-5,-4],[-3,-2],[1,2],[3,5],[8,9]], toBeRemoved = [-1,4]
**Output:** [[-5,-4],[-3,-2],[4,5],[8,9]]

**Constraints:**

-   `1 <= intervals.length <= 104`
-   `-109  <= ai  < bi  <= 109`

# Solution 1: O(n)
```
class Solution {
    public List<List<Integer>> removeInterval(int[][] intervals, int[] toBeRemoved) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        int index = 0, n = intervals.length;
        
        while(index < n && intervals[index][1] < toBeRemoved[0]) {
            List<Integer> arr = new ArrayList<Integer>();
            arr.add(intervals[index][0]);
            arr.add(intervals[index][1]);
            res.add(arr);
            index++;
        }
        
        while(index < n && intervals[index][0] < toBeRemoved[1]){
            if(intervals[index][0] < toBeRemoved[0]) {
                List<Integer> arr = new ArrayList<Integer>();
                arr.add(intervals[index][0]);
                arr.add(toBeRemoved[0]);
                res.add(arr);
            }
            
            if(intervals[index][1] > toBeRemoved[1]){
                List<Integer> arr = new ArrayList<Integer>();
                arr.add(toBeRemoved[1]);
                arr.add(intervals[index][1]);
                res.add(arr);
            }
            
            index++;
        }
        
        while(index < n) {
            List<Integer> arr = new ArrayList<Integer>();
            arr.add(intervals[index][0]);
            arr.add(intervals[index][1]);
            res.add(arr);
            index++;
        }
        
        
        return res;
    }
}
```

# Solution 2: O(n)
```
class Solution {
    public List<List<Integer>> removeInterval(int[][] intervals, int[] toBeRemoved) {
        List<List<Integer>> ans = new ArrayList<>();
        if(intervals.length == 0) return ans; // base case
        for (int[] interval : intervals) {
            if (toBeRemoved[0] >= interval[1] || toBeRemoved[1] <= interval[0]) {
                List<Integer> orig = new ArrayList<>();
                for (int num : interval) orig.add(num);
                ans.add(orig);
            }
            else {
                if (interval[0] < toBeRemoved[0]) {
                    List<Integer> leftHalf = new ArrayList<>();
                    leftHalf.add(interval[0]);
                    leftHalf.add(toBeRemoved[0]);
                    ans.add(leftHalf);
                }
                if (interval[1] > toBeRemoved[1]) {
                    List<Integer> rightHalf = new ArrayList<>();
                    rightHalf.add(toBeRemoved[1]);
                    rightHalf.add(interval[1]);
                    ans.add(rightHalf);
                }
            }
        }
        return ans;
    }
}
```