# 298. Binary Tree Longest Consecutive Sequence
Given the  `root`  of a binary tree, return  _the length of the longest consecutive sequence path_.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path needs to be from parent to child (cannot be the reverse).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/consec1-1-tree.jpg)

**Input:** root = [1,null,3,2,4,null,null,null,5]
**Output:** 3
**Explanation:** Longest consecutive sequence path is 3-4-5, so return 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/consec1-2-tree.jpg)

**Input:** root = [2,null,3,2,null,1]
**Output:** 2
**Explanation:** Longest consecutive sequence path is 2-3, not 3-2-1, so return 2.

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 3 * 104]`.
-   `-3 * 104  <= Node.val <= 3 * 104`

# Solution 1: Iterative Pre-order (Beat 5%)
```
class Solution {
    
    class Node{
        TreeNode treenode;
        int len;
        
        public Node(TreeNode _treenode, int _len){
            this.treenode = _treenode;
            this.len = _len;
        }
    }
    
    public int longestConsecutive(TreeNode root) {
        int res = 0;
        if(root == null) return 0;
        Deque<Node> stack = new ArrayDeque<Node>();
        stack.push(new Node(root, 1));
        
        while(!stack.isEmpty()){
            Node cur = stack.pop();
            res = Math.max(cur.len,res);
            if(cur.treenode.right != null){
                if(cur.treenode.right.val == cur.treenode.val + 1) 
                    stack.push(new Node(cur.treenode.right, cur.len+1)); 
                else
                    stack.push(new Node(cur.treenode.right, 1)); 
            }
            if(cur.treenode.left != null){
                if(cur.treenode.left.val == cur.treenode.val + 1) 
                    stack.push(new Node(cur.treenode.left, cur.len+1)); 
                else
                    stack.push(new Node(cur.treenode.left, 1)); 
            }
        }
        
        return res;
    }
}
``` 

# Solution 2: Recursive Pre-order (Beat 85%)
```
class Solution {
    int res = 1;
    public int longestConsecutive(TreeNode root) {
        if(root == null) return 0;
        dfs(root,1);
        return res;
    }
    
    public void dfs(TreeNode root, int len){
        if(root == null) return;
        res = Math.max(len, res);
        if(root.left != null && root.left.val == root.val + 1) dfs(root.left, len+1);
        else dfs(root.left, 1);
        
        if(root.right != null && root.right.val == root.val + 1) dfs(root.right, len+1);
        else dfs(root.right, 1);
    }
}
```