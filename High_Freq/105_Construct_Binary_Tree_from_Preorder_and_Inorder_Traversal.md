# 105. Construct Binary Tree from Preorder and Inorder Traversal
Given preorder and inorder traversal of a tree, construct the binary tree.

**Note:**  
You may assume that duplicates do not exist in the tree.

For example, given

preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]

Return the following binary tree:
```
    3
   / \
  9  20
    /  \
   15   7
```
# Solution 1: HashMap + DFS Recursion
```
class Solution {
    HashMap<Integer, Integer> in_map = new HashMap<Integer,Integer>();
    int index = 0;
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        for(int i = 0; i < inorder.length; i++) in_map.put(inorder[i],i);
        index = postorder.length - 1;
        return helper(inorder, postorder, 0, inorder.length - 1);
    }
    
    public TreeNode helper(int[] inorder, int[] postorder, int inL, int inR){
        if(index < 0 || inL > inR) return null;
        TreeNode root = new TreeNode(postorder[index--]);
        int pos =  in_map.get(root.val);
        root.right = helper(inorder, postorder, pos+1, inR);
        root.left = helper(inorder, postorder, inL, pos -1);
        
        return root;
    }
}
```

# Solution 2: Map + Iteration
```
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if(preorder.length == 0) return null;
        HashMap<Integer,Integer> map = new HashMap<>();
        for(int i = 0; i < inorder.length; i++) map.put(inorder[i],i);
        int index = 0;
        TreeNode root = new TreeNode(preorder[index++]);
        Stack<TreeNode> stack = new Stack();
        stack.push(root);
        while(index < preorder.length){
            TreeNode cur = new TreeNode(preorder[index++]);
            TreeNode prev = stack.peek();
            if(map.get(prev.val) > map.get(cur.val)){
                prev.left = cur;
            }else{
                while(!stack.isEmpty() && map.get(stack.peek().val) < map.get(cur.val)) prev = stack.pop();
                prev.right = cur;
            }
            stack.push(cur);

        }
        return root;
    }
}
```

# Solution 3: O(1) extra Space Recursion
```
class Solution {
    private int pre = 0, in = 0, n = 0;
    
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        n = preorder.length;
        return dfs(preorder, inorder, Integer.MIN_VALUE);
    }
    
    public TreeNode dfs(int[] preorder, int[] inorder, int stop){
        if(pre == n || in == n) return null;
        if(inorder[in] == stop) {
            in++;
            return null;
        }
        TreeNode root = new TreeNode(preorder[pre++]);
        root.left = dfs(preorder, inorder, root.val);
        root.right = dfs(preorder, inorder, stop); 
        return root;
    }
}
``` 