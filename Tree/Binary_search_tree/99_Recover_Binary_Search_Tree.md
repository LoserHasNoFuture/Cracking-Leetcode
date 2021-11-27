# 99. Recover Binary Search Tree
You are given the  `root`  of a binary search tree (BST), where the values of  **exactly**  two nodes of the tree were swapped by mistake.  _Recover the tree without changing its structure_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/28/recover1.jpg)

**Input:** root = [1,3,null,null,2]
**Output:** [3,1,null,null,2]
**Explanation:** 3 cannot be a left child of 1 because 3 > 1. Swapping 1 and 3 makes the BST valid.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/28/recover2.jpg)

**Input:** root = [3,1,4,null,null,2]
**Output:** [2,1,4,null,null,3]
**Explanation:** 2 cannot be in the right subtree of 3 because 2 < 3. Swapping 2 and 3 makes the BST valid.

**Constraints:**

-   The number of nodes in the tree is in the range  `[2, 1000]`.
-   `-231  <= Node.val <= 231  - 1`

**Follow up:** A solution using `O(n)` space is pretty straight-forward. Could you devise a constant `O(1)` space solution?

# Solution 1: Inorder bst
```
class Solution {
    TreeNode first = null, second = null, prev = null;
    public void recoverTree(TreeNode root) {
        dfs(root);
        int tmp = first.val;
        first.val = second.val;
        second.val = tmp;
    }
    
    public void dfs(TreeNode root){
        if(root == null) return;
        dfs(root.left);
        if(prev != null && root.val < prev.val){
            if(first == null) first = prev;
            second = root;
        }
        prev = root;
        dfs(root.right);
    }
}
```