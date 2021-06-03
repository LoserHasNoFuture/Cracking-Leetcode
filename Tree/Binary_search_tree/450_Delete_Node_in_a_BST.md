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
        TreeNode newHead = new TreeNode(1);
        newHead.left = root;    
        delete_key(newHead,root,key);
        return newHead.left;
    }
    
    public void delete_key(TreeNode prev, TreeNode root, int key){
        if(root == null) return;
        if(root.val == key) {
            delete_root(prev,root);
        }else if(root.val > key) delete_key(root,root.left,key);
        else delete_key(root,root.right,key);
    }
    
    public void delete_root(TreeNode prev,TreeNode root){
        if(root.left == null && root.right == null){
            if(prev.left == root) prev.left = null;
            else prev.right = null;
        }else if (root.left == null){
            if(prev.left == root) prev.left = root.right;
            else prev.right = root.right;
        }else if(root.right == null){
            if(prev.left == root) prev.left = root.left;
            else prev.right = root.left;
        }else{
            int smallest = find_and_delete_smallest(root,root.right);
            root.val = smallest;
        }
    }
    
    public int find_and_delete_smallest(TreeNode prev,TreeNode root){
        if(root.left != null) return find_and_delete_smallest(root,root.left);
        int val = root.val;
        delete_root(prev,root);
        return val;
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