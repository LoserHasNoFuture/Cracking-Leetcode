# 124. Binary Tree Maximum Path Sum
Given a  **non-empty**  binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain  **at least one node**  and does not need to go through the root.

**Example 1:**

**Input:** [1,2,3]
```
       1
      / \
     2   3
```
**Output:** 6

**Example 2:**

**Input:** [-10,9,20,null,null,15,7]
```
   -10
   / \
  9  20
    /  \
   15   7
```
**Output:** 42

# Solution 1: DFS Recursion
```
class Solution {
    int max = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        if(root == null) return 0;
        dfs(root);
        return max;
    }
    
    public int dfs(TreeNode root){
        if(root == null) return 0;
        int left = dfs(root.left);
        int right = dfs(root.right);

        left = left < 0? 0:left;
        right = right < 0? 0:right;
        max = Math.max(left+right+root.val,max);
        
        return Math.max(left,right)+root.val;
    }
    
}
```

# Solution 2: PostOrder Iteration + HashMap
```
class Solution {
    
    public int maxPathSum(TreeNode root) {
        if(root == null) return 0;
        HashMap<TreeNode, Integer> map = new HashMap<>();
        Stack<TreeNode> stack = new Stack();
        int res = Integer.MIN_VALUE;
        TreeNode cur = root;
        
        TreeNode prev = null;
        while(cur != null || !stack.isEmpty()){            
            while(cur != null){
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.peek();
            if(cur.right != null && cur.right != prev){
                cur = cur.right;
                continue;
            }
            cur = stack.pop();
            int left = Math.max(0,map.getOrDefault(cur.left,0));
            int right = Math.max(0,map.getOrDefault(cur.right,0));
            res = Math.max(left+right+cur.val,res);
            map.put(cur, Math.max(left,right)+cur.val);
            prev = cur;
            cur = null;
        }
        
        return res;
    }   
}
```