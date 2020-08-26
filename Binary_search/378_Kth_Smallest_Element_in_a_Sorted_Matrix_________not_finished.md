# 378. Kth Smallest Element in a Sorted Matrix
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

# Solution 1: Minheap O(N + klgN)
Refer from: [https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/discuss/85173/Share-my-thoughts-and-Clean-Java-Code](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/discuss/85173/Share-my-thoughts-and-Clean-Java-Code)
Here is the step of my solution:

1.  Build a minHeap of elements from the first row.
2.  Do the following operations k-1 times :  
    Every time when you poll out the root(Top Element in Heap), you need to know the row number and column number of that element(so we can create a tuple class here), replace that root with the next element from the same column.
```
class Solution {
    public int kthSmallest(int[][] mat, int k) {
        PriorityQueue<Node> pq = new PriorityQueue<>((a,b)->a.val-b.val);
        int len = mat.length;
        for(int i = 0; i < len; i++){
            Node node = new Node(0,i,mat[0][i]);    
            pq.offer(node);
        }
        
        while(k > 0){
            k--;
            Node node = pq.poll();
            if(k == 0) return node.val;
            if(node.row + 1 < len) {
                node = new Node(node.row+1,node.col,mat[node.row+1][node.col]);
                pq.offer(node);
            }
        }
        
        return -1;
    }
    
    class Node{
        int row;
        int col;
        int val;
        
        public Node(int row, int col, int val){
            this.row = row;
            this.col = col;
            this.val = val;
        }
    }
}
```

# Solution 2: Binary Search O(n log(n) log(max-min))
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
