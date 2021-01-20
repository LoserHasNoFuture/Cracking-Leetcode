# 897. Increasing Order Search Tree
Given a binary search tree, rearrange the tree in  **in-order**  so that the leftmost node in the tree is now the root of the tree, and every node has no left child and only 1 right child.

**Example 1:**
**Input:** [5,3,6,2,4,null,8,1,null,null,null,7,9]

       5
      / \
    3    6
   / \    \
  2   4    8
 /        / \ 
1        7   9

**Output:** [1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]

 1
  \
   2
    \
     3
      \
       4
        \
         5
          \
           6
            \
             7
              \
               8
                \
                 9  

**Constraints:**

-   The number of nodes in the given tree will be between  `1`  and  `100`.
-   Each node will have a unique integer value from  `0`  to  `1000`.

# Solution 1: DFS Recursion
Build a new Tree:
```
class Solution {
    TreeNode res = new TreeNode(5);
    TreeNode cur = res;
    public TreeNode increasingBST(TreeNode root) {
        inorder(root);
        // Trick: add a fake root
        return res.right;
    }
    
    public void inorder(TreeNode root){
        if(root == null) return;
        inorder(root.left);
        cur.right = new TreeNode(root.val);
        cur = cur.right;
        inorder(root.right);
    }
}
```

Modify the existing Tree:
```
class Solution {
    public TreeNode increasingBST(TreeNode root) {
        return dfs(root);
    }
    
    public TreeNode dfs(TreeNode root){
        if(root == null) return null;
        
        TreeNode left = dfs(root.left);
        root.left = null;
        if(left != null){
            TreeNode temp = left;
            while(temp.right != null) temp = temp.right;
            temp.right = root;
        }
        
        TreeNode right = dfs(root.right);
        root.right = right;
        
        return left == null?root:left;
    }
}
```

# Solution: InOrder (Using Stack)
```
class Solution {
    public TreeNode increasingBST(TreeNode root) {
        if(root == null) return root;
        TreeNode res = new TreeNode(0);
        Stack<TreeNode> stack = new Stack();
        
        TreeNode prev = res;
        TreeNode cur = root;
        while(!stack.isEmpty() || cur != null){
            while(cur != null){
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.pop();
            cur.left = null;
            prev.right = cur;
            prev = cur;
            cur = cur.right;
        }
        
        return res.right;
    }
}
```

# Solution: Morris Traverlsal (InOrder)
```
class Solution {
    public TreeNode increasingBST(TreeNode root) {
        if(root == null) return root;
        TreeNode res = new TreeNode(0);
        TreeNode father = res;
        
        while(root != null){
            if(root.left == null){
                father.right = root;
                father = root;
                root = root.right;
            }else{
                TreeNode prev = root.left;
                while(prev.right != null && prev.right != root) prev = prev.right;
                if(prev.right == null){
                    prev.right = root;
                    root = root.left;
                }else{
                    prev.right = null;
                    root.left = null;
                    father.right = root;
                    father = root;
                    root = root.right;
                }
            }
        }
        
        return res.right;
    }
}
```