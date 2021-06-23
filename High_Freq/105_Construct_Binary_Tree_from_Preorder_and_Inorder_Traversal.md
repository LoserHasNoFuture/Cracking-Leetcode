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
# Solution 1: DFS Recursion
```
class Solution {
    int index = 0;
    HashMap<Integer,Integer> map = new HashMap<>();
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for(int i = 0; i < inorder.length; i++) map.put(inorder[i],i);
        return buildTree(preorder,inorder,0,inorder.length -1);
    }
    
    public TreeNode buildTree(int[] preorder, int[] inorder, int inStart, int inEnd){
        if(index == preorder.length || inStart > inEnd) return null;
        
        TreeNode root = new TreeNode(preorder[index++]);
        int pos = map.get(root.val);
        root.left = buildTree(preorder, inorder, inStart, pos-1);
        root.right = buildTree(preorder, inorder, pos+1,inEnd);
        return root;
    }
}
```

# Solution 2: Iteration
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