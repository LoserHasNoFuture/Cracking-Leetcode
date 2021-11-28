# 1372. Longest ZigZag Path in a Binary Tree
You are given the  `root`  of a binary tree.

A ZigZag path for a binary tree is defined as follow:

-   Choose  **any** node in the binary tree and a direction (right or left).
-   If the current direction is right, move to the right child of the current node; otherwise, move to the left child.
-   Change the direction from right to left or from left to right.
-   Repeat the second and third steps until you can't move in the tree.

Zigzag length is defined as the number of nodes visited - 1. (A single node has a length of 0).

Return  _the longest  **ZigZag**  path contained in that tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/22/sample_1_1702.png)

**Input:** root = [1,null,1,1,1,null,null,1,1,null,1,null,null,null,1,null,1]
**Output:** 3
**Explanation:** Longest ZigZag path in blue nodes (right -> left -> right).

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/01/22/sample_2_1702.png)

**Input:** root = [1,1,1,null,1,null,null,1,1,null,1]
**Output:** 4
**Explanation:** Longest ZigZag path in blue nodes (left -> right -> left -> right).

**Example 3:**

**Input:** root = [1]
**Output:** 0

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 5 * 104]`.
-   `1 <= Node.val <= 100`

# Solution 1: DFS
```
class Solution {
    
    class Path{
        int left_len;
        int right_len;
        
        public Path(int _left_len, int _right_len){
            this.left_len = _left_len;
            this.right_len = _right_len;
        }
    }
    
    int res = 0;
    public int longestZigZag(TreeNode root) {
        dfs(root);
        return res;
    }
    
    public Path dfs(TreeNode root){
        if(root == null) return new Path(0,0);
        Path left = dfs(root.left);
        Path right = dfs(root.right);
     
        int l = 0, r = 0;
        if(root.left != null) l = left.right_len + 1;
        if(root.right != null) r = right.left_len + 1;
        
        res = Math.max(l,res);
        res = Math.max(r,res);
        
        return new Path(l,r);
    }
}
```
Another way of coding:
```
class Solution {
    
    int res = 0;
    public int longestZigZag(TreeNode root) {
        dfs(root, true);
        return res;
    }
    
    public int dfs(TreeNode root, boolean isLeft){
        if(root == null) return -1;
        
        int l = dfs(root.left, true);
        int r = dfs(root.right, false);
        
        res = Math.max(1+l, res);
        res = Math.max(1+r, res);
        
        return isLeft?r+1:l+1;
    }
}
```