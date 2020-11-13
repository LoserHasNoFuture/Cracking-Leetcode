# 314. Binary Tree Vertical Order Traversal
Given a binary tree, return the  _vertical order_  traversal of its nodes' values. (ie, from top to bottom, column by column).

If two nodes are in the same row and column, the order should be from  **left to right**.

**Examples 1:**

**Input:** `[3,9,20,null,null,15,7]` 
```
   3
  /\
 /  \
 9  20
    /\
   /  \
  15   7 
```
**Output:**

[
  [9],
  [3,15],
  [20],
  [7]
]

**Examples 2:**

**Input:** `[3,9,8,4,0,1,7]`
```
     3
    /\
   /  \
   9   8
  /\  /\
 /  \/  \
 4  01   7 
```
**Output:**

[
  [4],
  [9],
  [3,0,1],
  [8],
  [7]
]

**Examples 3:**

**Input:** `[3,9,8,4,0,1,7,null,null,null,2,5]` (0's right child is 2 and 1's left child is 5)
```
     3
    /\
   /  \
   9   8
  /\  /\
 /  \/  \
 4  01   7
    /\
   /  \
   5   2
```
**Output:**

[
  [4],
  [9,5],
  [3,0,1],
  [8,2],
  [7]
]

# Solution: BFS
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