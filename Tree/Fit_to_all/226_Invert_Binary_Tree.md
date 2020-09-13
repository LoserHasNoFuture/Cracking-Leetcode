# 226. Invert Binary Tree
Invert a binary tree.

**Example:**

Input:

     4
   /   \
  2     7
 / \   / \
1   3 6   9

Output:

     4
   /   \
  7     2
 / \   / \
9   6 3   1

**Trivia:**  
This problem was inspired by  [this original tweet](https://twitter.com/mxcl/status/608682016205344768)  by  [Max Howell](https://twitter.com/mxcl):

> Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so f*** off.

# Solution 1: DFS Recursion
```
class Solution {
    public TreeNode invertTree(TreeNode root) {
        return dfs(root);
    }
    
    public TreeNode dfs(TreeNode root){
        if(root == null) return null;
        TreeNode temp = dfs(root.left);
        root.left = dfs(root.right);
        root.right = temp;
        return root;
    }
}
```

# Solution 2: BFS Iteration
```
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null) return null;
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        
        while(!queue.isEmpty()){
            TreeNode cur = queue.poll();
            TreeNode temp = cur.right;
            cur.right = cur.left;
            cur.left = temp;
            if(cur.left != null) queue.offer(cur.left);
            if(cur.right != null) queue.offer(cur.right);
        }
        
        return root;
    }
}
```

# Solution 3: PreOrder (PostOrder)
```
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null) return null;
        Stack<TreeNode> stack = new Stack<TreeNode>();
        stack.push(root);
        
        while(!stack.isEmpty()){
            TreeNode cur = stack.pop();
            TreeNode temp = cur.right;
            cur.right = cur.left;
            cur.left = temp;
            if(cur.left != null) stack.push(cur.left);
            if(cur.right != null) stack.push(cur.right);
        }
        
        return root;
    }
}
```

# Solution 4: InOrder
```
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null) return null;
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode cur = root;
        
        while(!stack.isEmpty() || cur != null){
            while(cur!=null){
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.pop();
            
            TreeNode temp = cur.right;
            cur.right = cur.left;
            cur.left = temp;
            
            cur = cur.left;
        }
        
        return root;
    }
}
```

# Solution 5: Morris Traversal (NIUBI)
```
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null) return null;
        // root node can not be reversed.
        TreeNode new_root = new TreeNode(0);
        new_root.left = root;
        TreeNode cur = new_root;
        
        while(cur != null){
            if(cur.left != null){
                TreeNode prev = cur.left;
                while(prev.right != null && prev.right != cur) prev = prev.right;
                if(prev.right == null){
                    prev.right = cur;
                    cur = cur.left;
                    continue;
                }else{
                    for(TreeNode temp = cur.left; ; temp = temp.left){
                        TreeNode swap_temp = temp.left;
                        temp.left = temp.right;
                        temp.right = swap_temp;
                        if(temp == prev) break;
                    }
                    prev.left = null;
                }
            }
            cur = cur.right;
        }
        
        return new_root.left;
    }
}
```