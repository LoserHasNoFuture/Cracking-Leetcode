# 116. Populating Next Right Pointers in Each Node 117
You are given a  **perfect binary tree** where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to  `NULL`.

Initially, all next pointers are set to  `NULL`.

**Follow up:**

-   You may only use constant extra space.
-   Recursive approach is fine, you may assume implicit stack space does not count as extra space for this problem.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/02/14/116_sample.png)

**Input:** root = [1,2,3,4,5,6,7]
**Output:** [1,#,2,3,#,4,5,6,7,#]
**Explanation:** Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.

**Constraints:**

-   The number of nodes in the given tree is less than  `4096`.
-   `-1000 <= node.val <= 1000`

# Solution 1: BFS
```
class Solution {
    public Node connect(Node root) {
        
        
        Queue<Node> queue = new LinkedList<>();
        if(root != null) queue.offer(root);
        
        Node cur;
        while(!queue.isEmpty()){
            int sz = queue.size();
            
            cur = queue.poll();
            if(cur.left != null) queue.offer(cur.left);
            if(cur.right != null) queue.offer(cur.right);
            for(int i = 0; i < sz -1; i++){
                Node next = queue.poll();
                cur.next = next;
                cur = cur.next;
                if(cur.left != null) queue.offer(cur.left);
                if(cur.right != null) queue.offer(cur.right);
            }
            
            
        }
        
        return root;
    }
}
```

# Solution 2: DFS
```
class Solution {
    public Node connect(Node root) {
        dfs(root,null);
        return root;
    }
    
    public void dfs(Node root, Node next){
        if(root == null) return;
        if(root.left != null) {
            root.left.next = root.right;
            root.right.next = next;
            dfs(root.left,root.right.left);
            if(next != null) dfs(root.right,next.left);
            else dfs(root.right,null);
        }
        
    }
}
```

# Solution 3: Simple Iterative
```
class Solution {
    public Node connect(Node root) {
        if(root == null) return root;
        Node cur = root;
        Node next = root.left;
        while(cur != null){
            if(cur.left != null) cur.left.next = cur.right;
            if(cur.next != null && cur.right != null) {
                cur.right.next = cur.next.left;
                cur = cur.next;
            }else{
                cur = next;
                if(next!=null) next = next.left;
            }
        }
        return root;
    }
}
```