# 144. Binary Tree Preorder Traversal
Given a binary tree, return the  _preorder_  traversal of its nodes' values.

**Example:**

**Input:** `[1,null,2,3]`
   1
    \
     2
    /
   3

**Output:** `[1,2,3]`

**Follow up:**  Recursive solution is trivial, could you do it iteratively?

# Solution 1: DFS Recursion
```
class Solution {
    List<Integer> res = new ArrayList<Integer>();
    public List<Integer> preorderTraversal(TreeNode root) {
        dfs(root);
        return res;
    }
    
    public void dfs(TreeNode root){
        if(root == null) return;
        res.add(root.val);
        dfs(root.left);
        dfs(root.right);
    }
}
```

# Solution 2: Using Stack (2 ways), similar to postOrder
### 1st way
```
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if(root == null) return res;
        Stack<TreeNode> stack = new Stack();
        
        stack.push(root);
        while(!stack.isEmpty()){
            TreeNode cur = stack.pop();
            res.add(cur.val);
            if(cur.right != null) stack.push(cur.right);
            if(cur.left != null) stack.push(cur.left);
        }
        return res;
    }
}
```

### 2nd way
```
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if(root == null) return res;
        Stack<TreeNode> stack = new Stack();
        
        TreeNode cur = root;
        while(cur != null || !stack.isEmpty()){
            if(cur != null){
                res.add(cur.val);
                stack.push(cur);
                cur = cur.left;
            }else cur = stack.pop().right;
        }
        
        return res;
    }
}
```

# Solution 3: Morris Traversal
```
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        
        while(root != null){
            if(root.left == null){
                res.add(root.val);
                root = root.right;
            }else{
                TreeNode prev = root.left;
                while(prev.right != null && prev.right != root) prev = prev.right;
                if(prev.right == null){
                    res.add(root.val);
                    prev.right = root;
                    root = root.left;
                }else{
                    prev.right = null;
                    root = root.right;
                }
            }
        }
        
        return res;   
    }
}
```