# 111. Minimum Depth of Binary Tree
Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

**Example:**

Given binary tree  `[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```
return its minimum depth = 2.

# Solution 1: BFS
```
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null) return 0;
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        int res = 1;
        while(!queue.isEmpty()){
            int sz = queue.size();
            
            for(int i = 0; i < sz; i++){
                TreeNode cur = queue.poll();
                if(cur.left == null && cur.right == null) return res;
                if(cur.left != null) queue.offer(cur.left);
                if(cur.right != null) queue.offer(cur.right);
            }
            res++;
        }
        return res;
    }
}
```

# Solution 2: Morris Traversal
```
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null) return 0;
        int res = Integer.MAX_VALUE;
        TreeNode cur = root;
        int height = 0;
        while(cur != null){
            if(cur.left == null){
                height++;
                if(cur.right == null) res = Math.min(height,res);
                cur = cur.right;
            }else{
                int count = 1;
                TreeNode prev = cur.left;
                while(prev.right != null && prev.right != cur) {
                    prev = prev.right;
                    count++;
                }
                if(prev.right == null){
                    prev.right = cur;
                    height++;
                    cur = cur.left;
                }else{
                    if(prev.left == null) res = Math.min(height,res);
                    for(int i = 0; i < count; i++) height--;
                    prev.right = null;
                    cur = cur.right;
                }
            }
        } 
        return res;
    }
}
```


# Solution 3: DFS Recursion (Easy)
# Solution 4: Stack Iteation (Easy, adding other info)