# 889. Construct Binary Tree from Preorder and Postorder Traversal
### 答案不唯一
Given two integer arrays,  `preorder`  and  `postorder`  where  `preorder`  is the preorder traversal of a binary tree of  **distinct**  values and  `postorder`  is the postorder traversal of the same tree, reconstruct and return  _the binary tree_.

If there exist multiple answers, you can  **return any**  of them.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/24/lc-prepost.jpg)

**Input:** preorder = [1,2,4,5,3,6,7], postorder = [4,5,2,6,7,3,1]
**Output:** [1,2,3,4,5,6,7]

**Example 2:**

**Input:** preorder = [1], postorder = [1]
**Output:** [1]

**Constraints:**

-   `1 <= preorder.length <= 30`
-   `1 <= preorder[i] <= preorder.length`
-   All the values of  `preorder`  are  **unique**.
-   `postorder.length == preorder.length`
-   `1 <= postorder[i] <= postorder.length`
-   All the values of  `postorder`  are  **unique**.
-   It is guaranteed that  `preorder`  and  `postorder`  are the preorder traversal and postorder traversal of the same binary tree.

# Solution 1: DFS
```
class Solution {
    
    int pre = 0, post = 0;
    public TreeNode constructFromPrePost(int[] preorder, int[] postorder) {
        return dfs(preorder, postorder);
    }
    
    public TreeNode dfs(int[] preorder, int[] postorder){
        if(pre == preorder.length || post == postorder.length) return null;
        TreeNode root = new TreeNode(preorder[pre++]);
        if(root.val == postorder[post]){
            post++;
            return root;
        } 
        root.left = dfs(preorder, postorder);
        if(root.val == postorder[post]){
            post++;
            return root;
        } 
        root.right = dfs(preorder, postorder);
        // if(post < postorder.length) System.out.println(root.val + ", " + postorder[post]);
        if(root.val == postorder[post]){
            post++;
            return root;
        } 
        return root;
    }
}
```

Better Coding Skill:
```
class Solution {
    
    int pre_index = 0, post_index = 0;
    public TreeNode constructFromPrePost(int[] pre, int[] post) {
        TreeNode root = new TreeNode(pre[pre_index]);
        pre_index++;
        if(root.val != post[post_index])
            root.left = constructFromPrePost(pre,post);
        if(root.val != post[post_index])
            root.right = constructFromPrePost(pre,post);
        post_index++;
        return root;
    }
    

}
```

# Solution: Use Stack
```
class Solution {
    
    int pre_index = 0, post_index = 0;
    public TreeNode constructFromPrePost(int[] pre, int[] post) {
        TreeNode root = new TreeNode(pre[pre_index]);
        Stack<TreeNode> stack = new Stack();
        stack.push(root);
        pre_index++;
        while(pre_index < pre.length){
            TreeNode cur = stack.peek();
            while(cur.val == post[post_index]){
                post_index++; stack.pop();
                cur = stack.peek();
            }
            
            TreeNode next = new TreeNode(pre[pre_index]);
            if(cur.left == null) cur.left = next;
            else cur.right = next;
            stack.push(next);
            pre_index++;
        }
        
        return root;
    }
}
```