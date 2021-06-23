# 98. Validate Binary Search Tree
Given the  `root`  of a binary tree,  _determine if it is a valid binary search tree (BST)_.

A  **valid BST**  is defined as follows:

-   The left subtree of a node contains only nodes with keys  **less than**  the node's key.
-   The right subtree of a node contains only nodes with keys  **greater than**  the node's key.
-   Both the left and right subtrees must also be binary search trees.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)

**Input:** root = [2,1,3]
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg)

**Input:** root = [5,1,4,null,null,3,6]
**Output:** false
**Explanation:** The root node's value is 5 but its right child's value is 4.

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 104]`.
-   `-231  <= Node.val <= 231  - 1`

# Solution 1: Inorder Traversal (Beat 100%)
```
class Solution {
    int val = Integer.MIN_VALUE;
    boolean res = true, first = true;
    public boolean isValidBST(TreeNode root) {
        if(root == null) return true;
        dfs(root);
        return res;
    }
    
    public void dfs(TreeNode root){
        if(res == false || root == null) return;
        dfs(root.left);
        if(!first && root.val <= val) {
            res = false;
            return;
        }
        first = false;
        val = root.val;
        dfs(root.right);
    } 
}
```

# Solution 2: StraightFoward Solution (Beat 100%)
```
class Solution {
    public boolean isValidBST(TreeNode root) {
        if(root == null) return true;
        if(!isValidBST(root.left) || !isValidBST(root.right)) return false;
        
//         find largest at left tree
        boolean left_satisfy = true;
        if(root.left != null){
            if(root.left.right == null) left_satisfy = root.left.val < root.val;
            else{
                TreeNode cur = root.left;
                while(cur.right != null) cur = cur.right;
                left_satisfy = cur.val < root.val;
            }
        }
        if(!left_satisfy) return false;
        
//         find least
        boolean right_satisfy = true;
        if(root.right != null){
            if(root.right.left == null) right_satisfy = root.right.val > root.val;
            else{
                TreeNode cur = root.right;
                while(cur.left != null) cur = cur.left;
                right_satisfy = cur.val > root.val;
            }
        }
        
        return  right_satisfy;
    }
    
}
```
