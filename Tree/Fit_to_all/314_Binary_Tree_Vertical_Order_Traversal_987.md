# 314. Binary Tree Vertical Order Traversal
Given the  `root`  of a binary tree, return  _**the vertical order traversal**  of its nodes' values_. (i.e., from top to bottom, column by column).

If two nodes are in the same row and column, the order should be from  **left to right**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/28/vtree1.jpg)

**Input:** root = [3,9,20,null,null,15,7]
**Output:** [[9],[3,15],[20],[7]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/28/vtree2-1.jpg)

**Input:** root = [3,9,8,4,0,1,7]
**Output:** [[4],[9],[3,0,1],[8],[7]]

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/01/28/vtree2.jpg)

**Input:** root = [3,9,8,4,0,1,7,null,null,null,2,5]
**Output:** [[4],[9,5],[3,0,1],[8,2],[7]]

**Example 4:**

**Input:** root = []
**Output:** []

**Constraints:**

-   The number of nodes in the tree is in the range  `[0, 100]`.
-   `-100 <= Node.val <= 100`

# Solution 1:  BFS
Use HashMap 
```
class Solution {
    class Node{
        TreeNode node;
        int col;
        
        public Node(TreeNode _node, int _col){
            this.node = _node;
            this.col = _col;
        }
    }
    
    public List<List<Integer>> verticalOrder(TreeNode root) {
        List<List<Integer>> res= new ArrayList<List<Integer>>();
        Deque<Node> queue = new ArrayDeque<Node>();
        HashMap<Integer, List<Integer>> map = new HashMap<Integer, List<Integer>>();
        int min = 0;
        
        if(root != null) queue.offer(new Node(root,0));
        while(!queue.isEmpty()){
            Node cur = queue.poll();
            min = Math.min(cur.col, min);
            if(map.containsKey(cur.col)){
                List<Integer> arr = map.get(cur.col);
                arr.add(cur.node.val);
            }else{
                List<Integer> arr = new ArrayList<Integer>();
                arr.add(cur.node.val);
                map.put(cur.col, arr);
            }
            if(cur.node.left != null) queue.offer(new Node(cur.node.left, cur.col-1));
            if(cur.node.right != null) queue.offer(new Node(cur.node.right, cur.col+1));
        }
        
        while(map.size()>0){
            if(map.containsKey(min)) {
                res.add(map.get(min));
                map.remove(min);
            }
            min++;
        }
        
        return res;
    }
}
```

Use offset, direct add to results:
```
class Solution {
    class Node{
        TreeNode node;
        int pos;

        Node(TreeNode _node, int _pos){
            this.node = _node;
            this.pos = _pos;
        }
    }
    
    public List<List<Integer>> verticalOrder(TreeNode root) {
        ArrayList<List<Integer>> res = new ArrayList<List<Integer>>();
        if(root == null) return res;
        
        Queue<Node> queue = new LinkedList<Node>();
        queue.offer(new Node(root,0));
        int start = 0;
        
        List<Integer> arr;
        while(!queue.isEmpty()){
            Node cur = queue.poll();
            if(cur.pos < start){
                start = cur.pos;
                arr = new ArrayList<Integer>();
                arr.add(cur.node.val);
                res.add(0,arr);    
            }else if(cur.pos - start >= res.size()){
                arr = new ArrayList<Integer>();
                arr.add(cur.node.val);
                res.add(arr);    
            }else{
                arr = res.get(cur.pos - start);
                arr.add(cur.node.val);
            }
                
            if(cur.node.left != null) queue.offer(new Node(cur.node.left,cur.pos-1));
            if(cur.node.right != null) queue.offer(new Node(cur.node.right,cur.pos+1));
            
        }
        
        
        return res;
    }
}
```