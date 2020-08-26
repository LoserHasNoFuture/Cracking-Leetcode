# 404 Sum of Left Leaves

Find the sum of all left leaves in a given binary tree.

**Example:**

        3
       / \
      9  20
        /  \
       15   7

There are two left leaves in the binary tree, with values **9** and **15** respectively. Return **24**.


### This can be solved through dfs, bfs, preorder, inorder and postorder traversal.

# Solution 1: DFS Recursion
```
class Solution {
    int res = 0;
    public int sumOfLeftLeaves(TreeNode root) {
        dfs(root,false);
        return res;
    }
    
    public void dfs(TreeNode root, boolean flag){
        if(root == null) return;
        dfs(root.left,true);
        if(flag && root.left == null && root.right == null) res += root.val;
        dfs(root.right,false);
    }
}
```
# Solution 2: BFS Iterative Approach
```
class Solution {
    
    public int sumOfLeftLeaves(TreeNode root) {
        int res = 0;
        
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        if(root != null) queue.offer(root);
        
        while(!queue.isEmpty()){
            TreeNode cur = queue.poll();
            if(cur.left != null){
                if(cur.left.left == null && cur.left.right == null) res += cur.left.val;
                else queue.offer(cur.left);
            }
            if(cur.right != null) queue.offer(cur.right);
        }
        
        return res;
    }
    
}
```

# Solution 3: Post-order Iterative Approach
```
class Solution {
    
    public int sumOfLeftLeaves(TreeNode root) {
        int res = 0;
        Stack<TreeNode> stack = new Stack();
        TreeNode cur = root;
        while(cur != null || !stack.isEmpty()){
            if(cur != null){
                stack.push(cur);
                cur = cur.right;
            }else{
                cur = stack.pop().left;
                if(cur != null && cur.right == null && cur.left == null) res += cur.val;
            }
        }     
        return res;
    }
    
}
```

# Solution 4: Inorder Morris Traversal
```
class Solution {
    
    public int sumOfLeftLeaves(TreeNode root) {
        int res = 0;
        TreeNode cur = root;
        
        while(cur != null){
            if(cur.left == null) cur = cur.right;
            else{
                TreeNode prev = cur.left;
                // The leaf node can be put here or down in there. 
                // Put in here is worse, as it incurs more checks
                // if(prev.left == null && prev.right == null) res += prev.val;
                while(prev.right != null && prev.right != cur) prev = prev.right;
                if(prev.right == null){
                    prev.right = cur;
                    if(cur.left != null && cur.left.left == null && cur.left.right == cur) res += cur.left.val;
                    cur = cur.left;
                }else{
                    prev.right = null;
                    cur = cur.right;
                }
            }
        }
        
        return res;
    }
    
}
```