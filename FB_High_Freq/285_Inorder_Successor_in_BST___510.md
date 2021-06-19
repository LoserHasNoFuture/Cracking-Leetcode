# 285. Inorder Successor in BST
Given the  `root`  of a binary search tree and a node  `p`  in it, return  _the in-order successor of that node in the BST_. If the given node has no in-order successor in the tree, return  `null`.

The successor of a node  `p`  is the node with the smallest key greater than  `p.val`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/01/23/285_example_1.PNG)

**Input:** root = [2,1,3], p = 1
**Output:** 2
**Explanation:** 1's in-order successor node is 2. Note that both p and the return value is of TreeNode type.

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/01/23/285_example_2.PNG)

**Input:** root = [5,3,6,2,4,null,null,1], p = 6
**Output:** null
**Explanation:** There is no in-order successor of the current node, so the answer is `null`.

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 104]`.
-   `-105  <= Node.val <= 105`
-   All Nodes will have unique values.

# Solution 1: Naive Inorder (Can be applied to any tree) (Beat 60%)
```
class Solution {
    TreeNode succ = null;
    boolean find = false;;
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        dfs(root,p);
        return succ;
    }
    
    public void dfs(TreeNode root, TreeNode p){
        if(root == null || succ != null) return;
        
        dfs(root.left,p);
        
        if(find){
            succ = root;
            find = false;
        }
        
        if(root == p) find = true;
        
        dfs(root.right,p);
        
    }
}
```

# Solution 2: Consider the Property of BST (Beat 100%)
Recursive:
```
class Solution {
    
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {        
        if (root == null) return null;

        if (root.val <= p.val) {
            return inorderSuccessor(root.right, p);
        } else {
            TreeNode left = inorderSuccessor(root.left, p);
            return (left != null) ? left : root;
        }
    }

}
```

Iterative:
```
class Solution {
    
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        TreeNode succ = null;
        while(root != null){
            if(root.val > p.val){
                succ = root;
                root = root.left;
            }else root = root.right;
        }
        return succ;
    }

}
```
