# 106. Construct Binary Tree from Inorder and Postorder Traversal 105
Given two integer arrays  `inorder`  and  `postorder`  where  `inorder`  is the inorder traversal of a binary tree and  `postorder`  is the postorder traversal of the same tree, construct and return  _the binary tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

**Input:** inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
**Output:** [3,9,20,null,null,15,7]

**Example 2:**

**Input:** inorder = [-1], postorder = [-1]
**Output:** [-1]

**Constraints:**

-   `1 <= inorder.length <= 3000`
-   `postorder.length == inorder.length`
-   `-3000 <= inorder[i], postorder[i] <= 3000`
-   `inorder`  and  `postorder`  consist of  **unique**  values.
-   Each value of  `postorder`  also appears in  `inorder`.
-   `inorder`  is  **guaranteed**  to be the inorder traversal of the tree.
-   `postorder`  is  **guaranteed**  to be the postorder traversal of the tree.

# Solution 1: Hashmap 
```

```


# Solution 2: O(1) Space 
```
class Solution {
    
    private int in = 0, post = 0, n = 0;
    
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        n = inorder.length; in = n-1; post = n-1;
        return dfs(inorder, postorder, Integer.MAX_VALUE);
    }
    
    public TreeNode dfs(int[] inorder, int[] postorder, int stop){
        if(in < 0 || post < 0) return null;
        if(inorder[in] == stop){
            in--;
            return null;
        }
        TreeNode root = new TreeNode(postorder[post--]);
        root.right = dfs(inorder, postorder, root.val);
        root.left = dfs(inorder, postorder, stop);
        return root;
    }
}
```