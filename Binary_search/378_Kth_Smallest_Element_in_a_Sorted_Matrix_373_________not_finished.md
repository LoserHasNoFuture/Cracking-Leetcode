# 378. Kth Smallest Element in a Sorted Matrix 373
Given a  _n_  x  _n_  matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

**Example:**
```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,
```
return 13.

**Note:**  
You may assume k is always valid, 1 ≤ k ≤ n2.

# Solution 1: Minheap O(N + klgN) Beat 55%
Refer from: [https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/discuss/85173/Share-my-thoughts-and-Clean-Java-Code](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/discuss/85173/Share-my-thoughts-and-Clean-Java-Code)
Here is the step of my solution:

1.  Build a minHeap of elements from the first row.
2.  Do the following operations k-1 times :  
    Every time when you poll out the root(Top Element in Heap), you need to know the row number and column number of that element(so we can create a tuple class here), replace that root with the next element from the same column.
```
public class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        PriorityQueue<Tuple> pq = new PriorityQueue<Tuple>();
        for(int j = 0; j <= n-1; j++) pq.offer(new Tuple(0, j, matrix[0][j]));
        for(int i = 0; i < k-1; i++) {
            Tuple t = pq.poll();
            if(t.x == n-1) continue;
            pq.offer(new Tuple(t.x+1, t.y, matrix[t.x+1][t.y]));
        }
        return pq.poll().val;
    }
}

class Tuple implements Comparable<Tuple> {
    int x, y, val;
    public Tuple (int x, int y, int val) {
        this.x = x;
        this.y = y;
        this.val = val;
    }
    
    @Override
    public int compareTo (Tuple that) {
        return this.val - that.val;
    }
}
```

# Solution 2: Binary Search O(n log(n) log(max-min)) Beat 100%
```
class Solution {
    public int kthSmallest(int[][] mat, int k) {
        int len = mat.length;
        int start = mat[0][0];
        int end = mat[len-1][len-1];
        
        while(start < end){
            int mid = (start + end) >>> 1;
            
            int count = 0;
            for(int i = 0; i < len; i++){
                count += smaller_number(mat[i],mid);
            }
            
            if(count < k) start = mid + 1;
            else end = mid;            
        }
        
        return start;
    }
    
    public int smaller_number(int[] row, int target){
        int start = 0;
        int end = row.length;
        while(start < end){
            int mid = (start + end) >>> 1;
            if(row[mid] <= target) start = mid + 1;
            else end = mid;
        }
        
        return start;
    }
    
}
```

# Solution 3: To be continued
Refer from:[https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/discuss/85170/O(n)-from-paper.-Yes-O(rows).](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/discuss/85170/O(n)-from-paper.-Yes-O(rows).)
