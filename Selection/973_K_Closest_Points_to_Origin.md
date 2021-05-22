# 973. K Closest Points to Origin
Given an array of  `points`  where  `points[i] = [xi, yi]`  represents a point on the  **X-Y**  plane and an integer  `k`, return the  `k`  closest points to the origin  `(0, 0)`.

The distance between two points on the  **X-Y**  plane is the Euclidean distance (i.e.,  `√(x1  - x2)2  + (y1  - y2)2`).

You may return the answer in  **any order**. The answer is  **guaranteed**  to be  **unique**  (except for the order that it is in).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/03/closestplane1.jpg)

**Input:** points = [[1,3],[-2,2]], k = 1
**Output:** [[-2,2]]
**Explanation:**
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]].

**Example 2:**

**Input:** points = [[3,3],[5,-1],[-2,4]], k = 2
**Output:** [[3,3],[-2,4]]
**Explanation:** The answer [[-2,4],[3,3]] would also be accepted.

**Constraints:**

-   `1 <= k <= points.length <= 104`
-   `-104  < xi, yi  < 104`

# Solution 1: Randomized Selection Algorithm (Quicksort Beat 98%)
```
class Solution {
    
    int cur = 0;
    int[][] res;
    public int[][] kClosest(int[][] points, int k) {
        res = new int[k][2];
        int len = points.length;
        int[][] dis = new int[len][2];
        
        for(int i = 0; i < points.length; i++){
            dis[i][0] = points[i][0]*points[i][0] + points[i][1]*points[i][1];
            dis[i][1] = i;
        }
        
        find_kth(points,dis,0,len-1,k);
        
        return res;
    }
    
    public void find_kth(int[][] points, int[][] dis, int start, int end, int k){
        if(start >= end || k <= 0) return;
        int mid = (start + end) >>> 1;
        swap(dis,start,mid);
        int l = start + 1, r = end;
        
        while(l <= r){
            while(l <= r && dis[start][0] >= dis[l][0]) l++;
            while(l <= r && dis[start][0] < dis[r][0]) r--;
            
            if(l <= r){
                swap(dis,r,l);
                r--;l++;
            }
        }
        
        swap(dis,r,start);
        if(r-start <= k) {
            insert_to_res(dis,points,start,r);
            if(cur == res.length) return;
            res[cur++] = points[dis[r][1]];
            
            find_kth(points,dis,r+1,end, k - r + start - 1);
        }else find_kth(points,dis,start,r-1, k);
    }
    
    public void insert_to_res(int[][] dis, int[][] points, int start, int end){
        while(start < end){
            res[cur++] = points[dis[start++][1]]; 
        }
    }
    
    
    public void swap(int[][] dis, int a, int b){
        int[] tmp = new int[]{dis[a][0],dis[a][1]};
        dis[a][0] = dis[b][0];
        dis[a][1] = dis[b][1];
        dis[b][0] = tmp[0];
        dis[b][1] = tmp[1];
    }
    
}
```

A selection summary: [https://leetcode.com/problems/k-closest-points-to-origin/discuss/220235/Java-Three-solutions-to-this-classical-K-th-problem.](https://leetcode.com/problems/k-closest-points-to-origin/discuss/220235/Java-Three-solutions-to-this-classical-K-th-problem.)

Better coding skill:
```
class Solution {
    
    public int[][] kClosest(int[][] points, int K) {
        int len =  points.length, l = 0, r = len - 1;
        while (l <= r) {
            int mid = helper(points, l, r);
            if (mid == K) break;
            if (mid < K) {
                l = mid + 1;
            } else {
                r = mid - 1;
            }
        }
        return Arrays.copyOfRange(points, 0, K);
    }

    private int helper(int[][] A, int l, int r) {
        int[] pivot = A[l];
        while (l < r) {
            while (l < r && compare(A[r], pivot) >= 0) r--;
            A[l] = A[r];
            while (l < r && compare(A[l], pivot) <= 0) l++;
            A[r] = A[l];
        }
        A[l] = pivot;
        return l;
    }

    private int compare(int[] p1, int[] p2) {
        return p1[0] * p1[0] + p1[1] * p1[1] - p2[0] * p2[0] - p2[1] * p2[1];
    }
    
}
```