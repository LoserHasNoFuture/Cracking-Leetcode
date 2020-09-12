# 1022. Sum of Root To Leaf Binary Numbers
Given a binary tree, each node has value  `0` or  `1`. Each root-to-leaf path represents a binary number starting with the most significant bit. For example, if the path is  `0 -> 1 -> 1 -> 0 -> 1`, then this could represent  `01101`  in binary, which is  `13`.

For all leaves in the tree, consider the numbers represented by the path from the root to that leaf.

Return the sum of these numbers.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/04/04/sum-of-root-to-leaf-binary-numbers.png)

**Input:** [1,0,1,0,1,0,1]
**Output:** 22
**Explanation:** (100) + (101) + (110) + (111) = 4 + 5 + 6 + 7 = 22

**Note:**

1.  The number of nodes in the tree is between  `1`  and  `1000`.
2.  node.val is  `0`  or  `1`.
3.  The answer will not exceed  `2^31 - 1`.

# Solution 1: DFS Recursion
```
class Solution {
    private int res = 0;
    public int sumRootToLeaf(TreeNode root) {
        dfs(root,0);
        return res;
    }
    
    public void dfs(TreeNode root, int cur){
        if(root == null) return;
        cur = cur * 2 + root.val;
        if(root.left == null && root.right == null) res += cur;
        dfs(root.left,cur);
        dfs(root.right,cur);
    }
}
```

# Solution 2: Morris Traversal (Non-intrusive, niubi)
```
class Solution {  
    public int sumRootToLeaf(TreeNode root) {
        if(root == null) return 0;
        int res = 0;
        
        TreeNode cur = root;
        int val = 0;
        while(cur != null){
            if(cur.left == null){
                val = val * 2 + cur.val;
                if(cur.right == null) res+= val;
                cur = cur.right;
            }else{
                TreeNode prev = cur.left; 
                int count = 1;
                while(prev.right != null && prev.right != cur) {
                    prev = prev.right;
                    count++;
                }
                if(prev.right == null){
                    val = val *2 + cur.val;
                    prev.right = cur;
                    cur = cur.left;
                }else{
                    if(prev.left == null) res += val;
                    for(int i = 0; i < count; i++) val = val/2;
                    prev.right = null;
                    cur = cur.right;
                }
                
            }
        }
        
        return res;
    }
}
```

# Solution 3: Iterative Traversal Using Stack (Intrusive)
```
class Solution {
    
    public int sumRootToLeaf(TreeNode root) {
        if(root == null) return 0;
        int res = 0;
        Stack<TreeNode> stack = new Stack();
        stack.push(root);
        
        
        while(!stack.isEmpty()){
            TreeNode cur = stack.pop();
            if(cur.left == null && cur.right == null) res+=cur.val;
            if(cur.left != null) {
                cur.left.val = cur.val*2 + cur.left.val;
                stack.push(cur.left);
            }
            if(cur.right != null) {
                cur.right.val = cur.val*2 + cur.right.val;
                stack.push(cur.right);
            }
            
        }
        
        return res;
    }
}
```