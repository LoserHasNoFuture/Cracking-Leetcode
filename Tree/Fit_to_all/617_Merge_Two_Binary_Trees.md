# 617. Merge Two Binary Trees
Given two binary trees and imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not.

You need to merge them into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of new tree.

**Example 1:**

**Input:** 
	Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
**Output:** 
Merged tree:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7

**Note:**  The merging process must start from the root nodes of both trees.

# Solution 1: Recursive DFS
Modified Tree1:
```
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        return dfs(t1,t2);
    }
    
    public TreeNode dfs(TreeNode root1, TreeNode root2){
        if(root1 == null && root2 == null) return null;
        if(root1 == null) return root2;
        if(root2 == null) return root1;
        
        root1.left = dfs(root1.left,root2.left);
        root1.val = root1.val + root2.val;
        root1.right = dfs(root1.right,root2.right);
        return root1;
    }
}
```
Add a new Tree:
```
class Solution {
    TreeNode root;
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        root = dfs(t1,t2);
        return root;
    }
    
    public TreeNode dfs(TreeNode root1, TreeNode root2){
        if(root1 == null && root2 == null) return null;
        if(root1 == null) return root2;
        if(root2 == null) return root1;
        
        TreeNode root = new TreeNode(root1.val + root2.val);
        root.left = dfs(root1.left,root2.left);
        root.right = dfs(root1.right,root2.right);
        return root;
    }
}
```

# Solution 2: BFS
Modify Tree1:
```
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if(t1 == null && t2 == null) return null;
        if(t1 == null) return t2;
        if(t2 == null) return t1;
        
        Queue<TreeNode> queue1 = new LinkedList<>();
        Queue<TreeNode> queue2 = new LinkedList<>();
        t1.val = t1.val + t2.val;
        queue1.offer(t1);
        queue2.offer(t2);
    
        while(!queue1.isEmpty() && !queue2.isEmpty()){
            TreeNode cur1 = queue1.poll();
            TreeNode cur2 = queue2.poll();
            if(cur2.left != null) {
                if(cur1.left == null) cur1.left = cur2.left;
                else{
                    cur1.left.val = cur1.left.val  + cur2.left.val;
                    queue1.offer(cur1.left);
                    queue2.offer(cur2.left);
                }
            }
            
            if(cur2.right != null) {
                if(cur1.right == null) cur1.right = cur2.right;
                else{
                    cur1.right.val = cur1.right.val  + cur2.right.val;
                    queue1.offer(cur1.right);
                    queue2.offer(cur2.right);
                }
            }    
        }

        return t1;
    }
    

}
```

Add a New Tree:
```
class Solution {
    TreeNode root;
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if(t1 == null && t2 == null) return null;
        if(t1 == null) return t2;
        if(t2 == null) return t1;
        
        Queue<TreeNode> queue = new LinkedList<>();
        Queue<TreeNode> queue1 = new LinkedList<>();
        Queue<TreeNode> queue2 = new LinkedList<>();
        queue1.offer(t1);
        queue2.offer(t2);
        root = new TreeNode(t1.val+t2.val);
        queue.offer(root);
        
        while(!queue1.isEmpty() && !queue2.isEmpty()){
            TreeNode cur = queue.poll();
            TreeNode cur1 = queue1.poll();
            TreeNode cur2 = queue2.poll();
            if(cur1.left == null || cur2.left == null){
                if(cur1.left == null) cur.left = cur2.left;
                else cur.left = cur1.left;
            }else{
                cur.left = new TreeNode(cur1.left.val + cur2.left.val);
                queue.offer(cur.left);
                queue1.offer(cur1.left);
                queue2.offer(cur2.left);
            }

            if(cur1.right == null || cur2.right == null){
                if(cur1.right == null) cur.right = cur2.right;
                else cur.right = cur1.right;
            }else{
                cur.right = new TreeNode(cur1.right.val + cur2.right.val);
                queue.offer(cur.right);
                queue1.offer(cur1.right);
                queue2.offer(cur2.right);
            }
        }
        
        return root;
    }

}
```

# Solution 3: PreOrder (Others are similar)
```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if(t1 == null && t2 == null) return null;
        if(t1 == null) return t2;
        if(t2 == null) return t1;
        
        Stack<TreeNode> stack1 = new Stack<>();
        Stack<TreeNode> stack2 = new Stack<>();
        t1.val = t1.val + t2.val;
        stack1.push(t1);
        stack2.push(t2);
        
        while(!stack1.isEmpty() && !stack2.isEmpty()){
            TreeNode cur1 = stack1.pop();
            TreeNode cur2 = stack2.pop();
            
            if(cur2.right != null){
                if(cur1.right == null) cur1.right = cur2.right;
                else{
                    cur1.right.val = cur1.right.val + cur2.right.val;
                    stack1.push(cur1.right);
                    stack2.push(cur2.right);
                }
            }
            
            if(cur2.left != null){
                if(cur1.left == null) cur1.left = cur2.left;
                else{
                    cur1.left.val = cur1.left.val + cur2.left.val;
                    stack1.push(cur1.left);
                    stack2.push(cur2.left);
                }
            }
            
            
        }
       
        return t1;
    }
}
```

# Morris Traversal... NONONO!!