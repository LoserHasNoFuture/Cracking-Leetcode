# 1602. Find Nearest Right Node in Binary Tree 1022 111 226
Given the  `root`  of a binary tree and a node  `u`  in the tree, return  _the  **nearest**  node on the  **same level**  that is to the  **right**  of_  `u`_, or return_  `null`  _if_ `u`  _is the rightmost node in its level_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/24/p3.png)

**Input:** root = [1,2,3,null,4,5,6], u = 4
**Output:** 5
**Explanation:** The nearest node on the same level to the right of node 4 is node 5.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2020/09/23/p2.png)**

**Input:** root = [3,null,4,2], u = 2
**Output:** null
**Explanation:** There are no nodes to the right of 2.

**Example 3:**

**Input:** root = [1], u = 1
**Output:** null

**Example 4:**

**Input:** root = [3,4,2,null,null,null,1], u = 4
**Output:** 2

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 105]`.
-   `1 <= Node.val <= 105`
-   All values in the tree are  **distinct**.
-   `u`  is a node in the binary tree rooted at  `root`.

# Solution 1: BFS
```
class Solution {
    public TreeNode findNearestRightNode(TreeNode root, TreeNode u) {
        if(root == null || root == u) return null;
        Queue<TreeNode> q = new LinkedList<TreeNode>();
        q.offer(root);
        
        while(!q.isEmpty()){
            int sz = q.size();
            
            for(int i = 0; i < sz; i++){
                TreeNode cur = q.poll();
                
                if(cur == u){
                    if(i == sz - 1) return null;
                    else return q.poll();
                }
                
                if(cur.left != null) q.offer(cur.left);
                if(cur.right != null) q.offer(cur.right);
            }
        }
        
        return null;
    }
}
```

# Solution 2: DFS
```
class Solution {
    private int height;
    private TreeNode res;
    public TreeNode findNearestRightNode(TreeNode root, TreeNode u) {
        if(root == null || root == u) return null;
        dfs(root,u, 0);
        return res;
    }
    
    public void dfs(TreeNode root, TreeNode u, int depth){
        if(res != null || root == null) return;
        if(root == u) {
            height = depth;
            return;
        }
        
        if(height != 0 && depth == height) {
            res = root;
            return;
        }
        
        dfs(root.left,u,depth+1);
        dfs(root.right,u,depth+1);
    }
}
```

# Solution 3: Morris Traversal
```
class Solution {
    public TreeNode findNearestRightNode(TreeNode root, TreeNode u) {
        if(root == null || root == u) return null;
        int depth = 0;
        int height = 0;
        
        while(root != null){
            if(root.left == null){
                if(height != 0 && height == depth) return root;
                if(root == u) height = depth;
                
                depth++;
                root = root.right;
            }else{
                int count = 1;
                TreeNode prev = root.left;
                while(prev.right != null && prev.right != root) {
                    prev = prev.right;
                    count++;
                }
                
                if(prev.right == null){
                    if(height != 0 && height == depth) return root;
                    if(root == u) height = depth;
                    
                    depth++;
                    prev.right = root;
                    root = root.left;
                }else{
                    while(count-- > 0) depth--;
                    
                    prev.right = null;
                    root = root.right;
                }
            }         
        }
        
        return null;
    }
    
}
```