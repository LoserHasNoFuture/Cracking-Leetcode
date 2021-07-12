# 295. Find Median from Data Stream
The  **median**  is the middle value in an ordered integer list. If the size of the list is even, there is no middle value and the median is the mean of the two middle values.

-   For example, for  `arr = [2,3,4]`, the median is  `3`.
-   For example, for  `arr = [2,3]`, the median is  `(2 + 3) / 2 = 2.5`.

Implement the MedianFinder class:

-   `MedianFinder()`  initializes the  `MedianFinder`  object.
-   `void addNum(int num)`  adds the integer  `num`  from the data stream to the data structure.
-   `double findMedian()`  returns the median of all elements so far. Answers within  `10-5`  of the actual answer will be accepted.

**Example 1:**

**Input**
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]
**Output**
[null, null, null, 1.5, null, 2.0]

**Explanation**
```
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0
```
**Constraints:**

-   `-105  <= num <= 105`
-   There will be at least one element in the data structure before calling  `findMedian`.
-   At most  `5 * 104`  calls will be made to  `addNum`  and  `findMedian`.

**Follow up:**

-   If all integer numbers from the stream are in the range  `[0, 100]`, how would you optimize your solution?
-   If  `99%`  of all integer numbers from the stream are in the range  `[0, 100]`, how would you optimize your solution?

# Solution 1: Binary Search Tree
Inspired by the follow up question of "230. Kth Smallest Element in a BST".

We add an extra attribute "left_subtree_cnt" to class TreeNode, which keeps track of the number of elements in left subtree.

```
class MedianFinder {

    class TreeNode{
        int val;
        int cnt; // number of elements that equals to val
        int left_subtree_cnt;  // number of elements in its left subtree
        TreeNode left;
        TreeNode right;
        
        public TreeNode(int _val){
            this.val = _val;
            this.cnt = 1;
        }
    }
    
    /** initialize your data structure here. */
    TreeNode root;
    int size;
    public MedianFinder() {
        root = null;
        size = 0;
    }
    
    public void addNum(int num) {
        size++;
        if(root == null) root = new TreeNode(num);
        else insert_to_tree(root, num);
    }
    
    public void insert_to_tree(TreeNode cur, int num){
        if(cur.val == num)  cur.cnt++;
        else if(cur.val < num){
            if(cur.right == null) cur.right = new TreeNode(num);
            
            else insert_to_tree(cur.right, num);
        }else{
            cur.left_subtree_cnt++;
            if(cur.left == null) cur.left = new TreeNode(num);
            else insert_to_tree(cur.left, num);
        }
    }
    
    public double findMedian() {
        if(size == 0) return 0;
        if(size == 1) return root.val;
        
        int val = find(root, (size+1)/2, 0);
        if(size%2 == 1) return (double)val;
        int val2 = find(root, (size+1)/2 + 1, 0);
        return ((double)(val + val2))/2;
    }
    
    public int find(TreeNode cur, int mid, int cnt){
        
        if(cur.left_subtree_cnt + cnt >= mid) return find(cur.left, mid, cnt);
        
        if(cur.left_subtree_cnt + cnt < mid && cur.left_subtree_cnt + cnt + cur.cnt >= mid) return cur.val;
        
        return find(cur.right,mid, cur.left_subtree_cnt + cnt + cur.cnt);
    }
    
    
}
```

# Solution 2: Two Heap Solution (Beat 14%)
Refer From: [https://leetcode.com/problems/find-median-from-data-stream/discuss/74047/JavaPython-two-heap-solution-O(log-n)-add-O(1)-find](https://leetcode.com/problems/find-median-from-data-stream/discuss/74047/JavaPython-two-heap-solution-O(log-n)-add-O(1)-find)
```
class MedianFinder {

    private PriorityQueue<Integer> large = new PriorityQueue<>(Collections.reverseOrder());
    private PriorityQueue<Integer> small = new PriorityQueue<>();
    private boolean even = true;

    public double findMedian() {
        if (even)
            return (large.peek() + small.peek()) / 2.0;
        else
            return large.peek();
    }

// By using this adding rule, it makes sure that the large pq has the first (0->2/k) elements.
// And the small pq has the second half of elements. (larger value)
    public void addNum(int num) {
        if (even) {
            small.offer(num);
            large.offer(small.poll());
        } else {
            large.offer(num);
            small.offer(large.poll());
        }
        even = !even;
    }
    
}
```

# Solution For Follow Up:
Refer from:
[https://leetcode.com/problems/find-median-from-data-stream/discuss/275207/Solutions-to-follow-ups](https://leetcode.com/problems/find-median-from-data-stream/discuss/275207/Solutions-to-follow-ups)
 _**1. If all integer numbers from the stream are between 0 and 100, how would you optimize it?**_

We can maintain an integer array of length 100 to store the count of each number along with a total count. Then, we can iterate over the array to find the middle value to get our median.

Time and space complexity would be O(100) = O(1).

_**2. If 99% of all integer numbers from the stream are between 0 and 100, how would you optimize it?**_

As 99% is between 0-100. So can keep a counter for less_than_hundred and greater_than_hundred.  
As we know soluiton will be definately in 0-100 we don't need to know those number which are >100 or <0, only count of them will be enough.