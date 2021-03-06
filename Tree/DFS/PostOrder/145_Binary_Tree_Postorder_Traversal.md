# 145. Binary Tree Postorder Traversal
Given a binary tree, return the  _postorder_  traversal of its nodes' values.

**Example:**

**Input:** `[1,null,2,3]`
   1
    \
     2
    /
   3

**Output:** `[3,2,1]`

**Follow up:**  Recursive solution is trivial, could you do it iteratively?

# Solution 1:  DFS Recursion
```
class Solution {
    List<Integer> res = new ArrayList<Integer>();
    public List<Integer> postorderTraversal(TreeNode root) {
        dfs(root);
        return res;    
    }
    
    public void dfs(TreeNode root){
        if(root == null) return;
        dfs(root.left);
        dfs(root.right);
        res.add(root.val);
    }
}
```

# Solution 2: Stack (3 ways)
### 1st way:
```
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if(root == null) return res;
        Stack<TreeNode> stack = new Stack();
        
        stack.push(root);
        while(!stack.isEmpty()){
            TreeNode cur = stack.pop();
            res.add(0,cur.val);
            if(cur.left != null) stack.push(cur.left);
            if(cur.right != null) stack.push(cur.right);
        }
        return res;    
    }
  }
```

### 2nd way:
```
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if(root == null) return res;
        Stack<TreeNode> stack = new Stack();
        
        TreeNode cur = root;
        while(!stack.isEmpty() || cur != null){
            if(cur != null){
                res.add(0,cur.val);
                stack.push(cur);
                cur = cur.right;
            }else cur = stack.pop().left;
        }    
        return res;    
    }  
}
```

### 3rd way:
```
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if(root == null) return res;
        Stack<TreeNode> stack = new Stack();
        
        TreeNode cur = root;
        TreeNode prev = null;
        while(!stack.isEmpty() || cur != null){
            if(cur != null){
                stack.push(cur);
                cur = cur.left;
            }else{
                cur = stack.peek();
                if(cur.right != null && cur.right != prev)
                    cur = cur.right;
                else{
                    stack.pop();
                    res.add(cur.val);
                    prev = cur;
                    cur = null;        
                }
            }
        }
        
        return res;    
    }  
}
```

### Morris Traversal
```
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if(root == null) return res;
        
        TreeNode cur = root;
        while(cur != null){
            if(cur.right == null){
                res.add(0, cur.val);
                cur = cur.left;
            }else{
                TreeNode prev = cur.right;
                while(prev.left != null && prev.left != cur) prev = prev.left;
                if(prev.left == null){
                    prev.left = cur;
                    res.add(0,cur.val);
                    cur = cur.right;
                }else{
                    prev.left = null;
                    cur = cur.left;
                }
            }
        }
        
        return res;    
    }  
}
```