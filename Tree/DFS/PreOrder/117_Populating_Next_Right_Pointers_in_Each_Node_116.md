# 117. Populating Next Right Pointers in Each Node II 116
Given a binary tree

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

![](https://assets.leetcode.com/uploads/2019/02/15/117_sample.png)

**Input:** root = [1,2,3,4,5,null,7]
**Output:** [1,#,2,3,#,4,5,7,#]
**Explanation:** Given the above binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.

**Constraints:**

-   The number of nodes in the given tree is less than  `6000`.
-   `-100 <= node.val <= 100`

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

# Solution 2: DFS, from right to left
```
class Solution {
    public Node connect(Node root) {
        dfs(root);
        return root;
    }
    
    public void dfs(Node root){
        if(root == null) return;
        if(root.right != null){
            if(root.left != null) root.left.next = root.right;
            root.right.next = find_next(root.next);
        }else if(root.left != null) root.left.next = find_next(root.next);
        
        // from right to left
        dfs(root.right);
        dfs(root.left);
        
    }
    
    public Node find_next(Node next){
        while(next != null){
            if(next.left != null) return next.left;
            if(next.right != null) return next.right;
            next = next.next;
        }
        return null;
    }
    
}
```

# Solution 3: Iterative Solution
```
class Solution {
    public Node connect(Node root) {
        if(root == null) return root;
        Node cur = root;
        Node next = null;
        while(cur != null){
            if(cur.left != null){
                if(next == null) next = cur.left;
                if(cur.right != null) cur.left.next = cur.right;
                else cur.left.next = find_next(cur.next);
            }
            if(cur.right != null){
                if(next == null) next = cur.right;
                cur.right.next = find_next(cur.next);
            }
            if(cur.next == null) {
                cur = next;
                next = null;
            }
            else cur = cur.next;
        }

        return root;
    }
    
    public Node find_next(Node root){
        if(root == null) return null;
        if(root.left != null) return root.left;
        if(root.right != null) return root.right;
        return find_next(root.next);
    }
}
```

Another way of writing:
```
class Solution {
    public Node connect(Node root) {
        Node p = new Node(0);
        Node cur = root;
        Node first =null;
        while(cur != null){
            if(cur.left != null){
                p.next = cur.left;
                p = p.next;
                if(first == null) first = cur.left;
            }
            if(cur.right != null){
                p.next = cur.right;
                p = p.next;
                if(first == null) first = cur.right;
            }
            if(cur.next != null){
                cur = cur.next;
            }else{
                cur = first;
                p = new Node(0);
                first = null;
            }
        }
        
        return root;
    }
}
```