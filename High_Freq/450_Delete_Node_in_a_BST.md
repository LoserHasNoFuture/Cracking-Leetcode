# 450. Delete Node in a BST
Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.

Basically, the deletion can be divided into two stages:

1.  Search for a node to remove.
2.  If the node is found, delete the node.

**Follow up:** Can you solve it with time complexity  `O(height of tree)`?

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/04/del_node_1.jpg)

**Input:** root = [5,3,6,2,4,null,7], key = 3
**Output:** [5,4,6,2,null,null,7]
**Explanation:** Given key to delete is 3. So we find the node with value 3 and delete it.
One valid answer is [5,4,6,2,null,null,7], shown in the above BST.
Please notice that another valid answer is [5,2,6,null,4,null,7] and it's also accepted.
![](https://assets.leetcode.com/uploads/2020/09/04/del_node_supp.jpg)

**Example 2:**

**Input:** root = [5,3,6,2,4,null,7], key = 0
**Output:** [5,3,6,2,4,null,7]
**Explanation:** The tree does not contain a node with value = 0.

**Example 3:**

**Input:** root = [], key = 0
**Output:** []

**Constraints:**

-   The number of nodes in the tree is in the range  `[0, 104]`.
-   `-105  <= Node.val <= 105`
-   Each node has a  **unique**  value.
-   `root`  is a valid binary search tree.
-   `-105  <= key <= 105`

# Solution 1: Recursive (Beat 100%)
```
// key idea:
// 1. if node is leaf node, direct delete
// 2. if node has only one child, move its child to replace it
// 3. Either find the smallest in right substree, or find the largest in left subtree (using bst property to speed up this process)

class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if(root == null) return null;
        if(root.val == key) return delete(root);
        if(root.val > key) root.left = deleteNode(root.left,key);
        else root.right = deleteNode(root.right,key);
        return root;
    }
    
    public TreeNode delete(TreeNode root){
        if(root.left == null) return root.right;
        if(root.right == null) return root.left;
        
        TreeNode slightly_greater = root.right;
        TreeNode prev = root;
        while(slightly_greater.left != null) {
            prev = slightly_greater;
            slightly_greater = slightly_greater.left;
        }
        root.val = slightly_greater.val;
        if(prev == root) root.right = delete(slightly_greater);
        else prev.left = delete(slightly_greater);
        return root;
    }
}
```

# Solution 2: Iterative (Beat 100%)
```
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        TreeNode res = new TreeNode(0);
        res.right = root;
        
        TreeNode prev = res;
        while(root != null && root.val != key){
            prev = root;
            if(root.val > key){
                root = root.left;
            }else if(root.val < key){
                root = root.right;
            }
        }
        if(prev.left == root) prev.left = deleteRoot(root);
        else prev.right = deleteRoot(root);
        return res.right;
    }
    
    public TreeNode deleteRoot(TreeNode root){
        if(root == null) return root;
        if(root.left == null) return root.right;
        if(root.right == null) return root.left;
        
        TreeNode cur = root.left;
        TreeNode prev = null;
        while(cur.right != null){
            prev = cur;
            cur = cur.right;
        }
        root.val = cur.val;
        if(prev != null) prev.right = cur.left;
        else root.left = cur.left;
        
        return root;
    }
}
```