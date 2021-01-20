# 938. Range Sum of BST
Given the  `root`  node of a binary search tree, return the sum of values of all nodes with value between  `L`  and  `R`  (inclusive).

The binary search tree is guaranteed to have unique values.

**Example 1:**

**Input:** root = [10,5,15,3,7,null,18], L = 7, R = 15
**Output:** 32

**Example 2:**

**Input:** root = [10,5,15,3,7,13,18,1,null,6], L = 6, R = 10
**Output:** 23

**Note:**

1.  The number of nodes in the tree is at most  `10000`.
2.  The final answer is guaranteed to be less than  `2^31`.

# Solution 1: DFS Recursion
```
class Solution {
    public int rangeSumBST(TreeNode root, int L, int R) {
        if(root == null) return 0;
        if(root.val > R) return rangeSumBST(root.left, L, R);
        if(root.val < L) return rangeSumBST(root.right, L, R);
        return root.val + rangeSumBST(root.left, L, R) + rangeSumBST(root.right, L, R);      
    }
}
```
# Solution 2: InOrder Stack
```
class Solution {    
    public int rangeSumBST(TreeNode root, int L, int R) {
        if(root == null) return 0;
        int res = 0;
        
        Stack<TreeNode> stack = new Stack();
        TreeNode cur = root;
        
        while(!stack.isEmpty() || cur != null){
            while(cur != null){
                stack.push(cur);
                if(cur.val > L) cur = cur.left;
                else break;
            }
            cur = stack.pop();
            if(cur.val > R) return res;
            if(cur.val >= L) res += cur.val;
            cur = cur.right;
        }
        
        return res;
    }
    
}
```

# Solution 3: Morris Traversal
```
class Solution {    
    public int rangeSumBST(TreeNode root, int L, int R) {
        int res = 0;
        
        while(root != null){
            if(root.left == null || root.val < L){
                if(root.val > R) return res;
                if(root.val >= L && root.val <= R) res += root.val;
                root = root.right;
            }else{
                TreeNode prev = root.left;
                while(prev.right != null && prev.right != root) prev = prev.right;
                if(prev.right == null){
                    prev.right = root;
                    root = root.left;
                }else{
                    prev.right = null;
                    if(root.val >= L && root.val <= R) res += root.val;
                    root = root.right;
                }
            }
        }
        
        return res;
    }
    
}
```

