# 1008. Construct Binary Search Tree from Preorder Traversal 449
Given an array of integers preorder, which represents the  **preorder traversal**  of a BST (i.e.,  **binary search tree**), construct the tree and return  _its root_.

It is  **guaranteed**  that there is always possible to find a binary search tree with the given requirements for the given test cases.

A  **binary search tree**  is a binary tree where for every node, any descendant of  `Node.left`  has a value  **strictly less than**  `Node.val`, and any descendant of  `Node.right`  has a value  **strictly greater than**  `Node.val`.

A  **preorder traversal**  of a binary tree displays the value of the node first, then traverses  `Node.left`, then traverses  `Node.right`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/03/06/1266.png)

**Input:** preorder = [8,5,1,7,10,12]
**Output:** [8,5,10,1,7,null,12]

**Example 2:**

**Input:** preorder = [1,3]
**Output:** [1,null,3]

**Constraints:**

-   `1 <= preorder.length <= 100`
-   `1 <= preorder[i] <= 1000`
-   All the values of  `preorder`  are  **unique**.

# Solution: DFS
```
class Solution {
    int index = 0;
    public TreeNode bstFromPreorder(int[] preorder) {
        return dfs(preorder, Integer.MAX_VALUE);
    }
    
    public TreeNode dfs(int[] preorder, int upperbound){
        if(index >= preorder.length || preorder[index] > upperbound) return null;
        TreeNode root = new TreeNode(preorder[index++]);
        root.left = dfs(preorder, root.val);
        root.right = dfs(preorder, upperbound);
        return root;
    }
}
```

Better coding:
```
class Solution {
    int index = 0;
    public TreeNode bstFromPreorder(int[] preorder) {
        return dfs(preorder, Integer.MAX_VALUE);
    }
    
    public TreeNode dfs(int[] preorder, int upperbound){
        if(index >= preorder.length || preorder[index] > upperbound) return null;
        TreeNode root = new TreeNode(preorder[index++]);
        root.left = dfs(preorder, root.val);
        root.right = dfs(preorder, upperbound);
        return root;
    }
}
```
