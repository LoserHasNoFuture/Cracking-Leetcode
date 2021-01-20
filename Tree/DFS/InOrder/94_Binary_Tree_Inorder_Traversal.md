# 94. Binary Tree Inorder Traversal
  
Given a binary tree, return the  _inorder_  traversal of its nodes' values.

**Example:**

**Input:** [1,null,2,3]
   1
    \
     2
    /
   3

**Output:** [1,3,2]

**Follow up:**  Recursive solution is trivial, could you do it iteratively?

# Solution 1: DFS Recursion
```
class Solution {
    List<Integer> res = new ArrayList<Integer>();
    public List<Integer> inorderTraversal(TreeNode root) {
        dfs(root);
        return res;
    }
    
    public void dfs(TreeNode root){
        if(root == null) return;
        dfs(root.left);
        res.add(root.val);
        dfs(root.right);
    }
}
```

# Solution 2: Using Stack
```
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        Stack<TreeNode> stack = new Stack();
        
        TreeNode cur = root;
        while(cur != null || !stack.isEmpty()){
            while(cur != null){
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.pop();
            res.add(cur.val);
            cur = cur.right;
        }
        
        return res;
    }
}
```

# Solution 3: Morris Traversal
```
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        Stack<TreeNode> stack = new Stack();
        
        TreeNode cur = root;
        while(cur != null){
            if(cur.left == null){
                res.add(cur.val);
                cur = cur.right;
            }else{
                TreeNode prev = cur.left;
                while(prev.right != null && prev.right != cur) prev = prev.right;
                if(prev.right == null){
                    prev.right = cur;
                    cur = cur.left;
                }else{
                    prev.right = null;
                    res.add(cur.val);
                    cur = cur.right;
                }
            }
        }
        
        return res;
    }
}
```