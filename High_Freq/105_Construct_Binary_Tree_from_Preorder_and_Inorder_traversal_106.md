# 105. Construct Binary Tree from Preorder and Inorder Traversal 106
Given two integer arrays  `preorder`  and  `inorder`  where  `preorder`  is the preorder traversal of a binary tree and  `inorder`  is the inorder traversal of the same tree, construct and return  _the binary tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

**Input:** preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
**Output:** [3,9,20,null,null,15,7]

**Example 2:**

**Input:** preorder = [-1], inorder = [-1]
**Output:** [-1]

**Constraints:**

-   `1 <= preorder.length <= 3000`
-   `inorder.length == preorder.length`
-   `-3000 <= preorder[i], inorder[i] <= 3000`
-   `preorder`  and  `inorder`  consist of  **unique**  values.
-   Each value of  `inorder`  also appears in  `preorder`.
-   `preorder`  is  **guaranteed**  to be the preorder traversal of the tree.
-   `inorder`  is  **guaranteed**  to be the inorder traversal of the tree.

# Solution 1: Hashmap 
use hashmap to record the position of each element in inorder
O(n) Space

# Solution 2: O(1) Space 
```
class Solution {
    private int in = 0;
    private int pre = 0;
    
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        return build(preorder, inorder, Integer.MIN_VALUE);
    }
    
    private TreeNode build(int[] preorder, int[] inorder, int stop) {
        if (pre >= preorder.length) return null;
        if (inorder[in] == stop) {
            in++;
            return null;
        }
        TreeNode node = new TreeNode(preorder[pre++]);
        node.left = build(preorder, inorder, node.val);
        node.right = build(preorder, inorder, stop);
        return node;        
    }
}
```