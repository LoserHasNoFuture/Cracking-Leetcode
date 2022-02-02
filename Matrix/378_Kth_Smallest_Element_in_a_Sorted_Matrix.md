# 378. Kth Smallest Element in a Sorted Matrix
Given an  `n x n`  `matrix`  where each of the rows and columns is sorted in ascending order, return  _the_  `kth`  _smallest element in the matrix_.

Note that it is the  `kth`  smallest element  **in the sorted order**, not the  `kth`  **distinct**  element.

You must find a solution with a memory complexity better than  `O(n2)`.

**Example 1:**

**Input:** matrix = [[1,5,9],[10,11,13],[12,13,15]], k = 8
**Output:** 13
**Explanation:** The elements in the matrix are [1,5,9,10,11,12,13,**13**,15], and the 8th smallest number is 13

**Example 2:**

**Input:** matrix = [[-5]], k = 1
**Output:** -5

**Constraints:**

-   `n == matrix.length == matrix[i].length`
-   `1 <= n <= 300`
-   `-109  <= matrix[i][j] <= 109`
-   All the rows and columns of  `matrix`  are  **guaranteed**  to be sorted in  **non-decreasing order**.
-   `1 <= k <= n2`

**Follow up:**

-   Could you solve the problem with a constant memory (i.e.,  `O(1)`  memory complexity)?
-   Could you solve the problem in  `O(n)`  time complexity? The solution may be too advanced for an interview but you may find reading  [this paper](http://www.cse.yorku.ca/~andy/pubs/X+Y.pdf)  fun.


1. 空间最优: 猜答案, 然后判断个数是否满足条件, 可以用两层binary search 
2. 时间最优: 有点类似于binary search的思路, 第一个点一定是最小的, 然后只往下走,和往右走. 
3. 时间最优: 优化2, 先把第一列加进去, 之后每一次取出来, 只需要加它的后面那个

# Solution 1: Binary Search
```
public class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int lo = matrix[0][0], hi = matrix[matrix.length - 1][matrix[0].length - 1] + 1;//[lo, hi)
        while(lo < hi) {
            int mid = lo + (hi - lo) / 2;
            int count = 0,  j = matrix[0].length - 1;
            // 找到小于等于它的个数
            for(int i = 0; i < matrix.length; i++) {
                j = find_smaller(matrix[i], j, mid);
                count += (j + 1);
            }
            if(count < k) lo = mid + 1;
            else hi = mid;
        }
        return lo;
    }
    
    
    public int find_smaller(int[] nums, int end, int target){
        int start = -1;
        
        while(start < end){
            int mid = (start + end + 1) >>> 1;
            if(nums[mid] > target) end = mid - 1;
            else start = mid;
        }
        
        return start;
    }
}
```

# Solution 2: BFS
```
public class Solution {
    public int kthSmallest(int[][] mat, int k) {
        int m = mat.length, n = mat[0].length;
        
        PriorityQueue<Node> pq = new PriorityQueue<Node>();
        pq.offer(new Node(0,0,mat[0][0]));
        boolean[][] visited = new boolean[m][n];
        visited[0][0] = true;
        
        
        int[][] neighbors = new int[][]{{1,0},{0,1}};
        while(!pq.isEmpty()){
            Node cur = pq.poll();
            if(--k == 0) return cur.val;
            
            for(int[] next: neighbors){
                int _x = cur.x + next[0];
                int _y = cur.y + next[1];
                
                if(_x < 0 || _y < 0 || _x >= m || _y >= n || visited[_x][_y]) continue;
                visited[_x][_y] = true;
                pq.offer(new Node(_x,_y,mat[_x][_y]));
            }
            
        }
        
        System.out.println("Error!!");
        return -1;
    }
    
    
    class Node implements Comparable<Node>{
        int x;
        int y;
        int val;
        
        Node(int _x, int _y, int _val){
            this.x = _x;
            this.y = _y;
            this.val = _val;
        }
        
        @Override
        public int compareTo(Node that){
            return this.val - that.val;
        }
    }
}
```

# Solution 3: Optimized from 2
```
class Solution { // 18 ms, faster than 32.44%
    public int kthSmallest(int[][] matrix, int k) {
        int m = matrix.length, n = matrix[0].length, ans = -1; // For general, the matrix need not be a square
        PriorityQueue<int[]> minHeap = new PriorityQueue<>(Comparator.comparingInt(o -> o[0]));
        for (int r = 0; r < Math.min(m, k); ++r)
            minHeap.offer(new int[]{matrix[r][0], r, 0});

        for (int i = 1; i <= k; ++i) {
            int[] top = minHeap.poll();
            int r = top[1], c = top[2];
            ans = top[0];
            if (c + 1 < n) minHeap.offer(new int[]{matrix[r][c + 1], r, c + 1});
        }
        return ans;
    }
}
```