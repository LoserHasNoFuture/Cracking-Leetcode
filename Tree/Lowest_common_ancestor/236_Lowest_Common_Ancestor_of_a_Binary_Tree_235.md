# 236. Lowest Common Ancestor of a Binary Tree 235
Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the  [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes  `p`  and  `q`  as the lowest node in  `T`  that has both  `p`  and  `q`  as descendants (where we allow  **a node to be a descendant of itself**).”

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Input:** root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
**Output:** 3
**Explanation:** The LCA of nodes 5 and 1 is 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Input:** root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
**Output:** 5
**Explanation:** The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.

**Example 3:**

**Input:** root = [1,2], p = 1, q = 2
**Output:** 1

**Constraints:**

-   The number of nodes in the tree is in the range  `[2, 105]`.
-   `-109  <= Node.val <= 109`
-   All  `Node.val`  are  **unique**.
-   `p != q`
-   `p`  and  `q`  will exist in the tree.


# Solution 1: DFS Recursion (Beat 100%)
```
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null) return null;
        if(root == p || root == q) return root;
        
        TreeNode left = lowestCommonAncestor(root.left,p,q);
        TreeNode right = lowestCommonAncestor(root.right,p,q);
        
        if(left == null && right == null) return null;
        if(left != null && right != null) return root;
        
        return left == null? right:left;
    }
}
```

# Solution 2: PostOrder Iteration
```
public class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        Map<TreeNode, TreeNode> map = new HashMap<>();
        Deque<TreeNode> stack = new ArrayDeque<>();
        TreeNode cur = root, prev = null;
        
        while (!stack.isEmpty() || cur != null) {
            while(cur != null){
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.peek();
            if(cur.right != null && cur.right != prev) {
                cur = cur.right;
                continue;
            }
            stack.pop();
            
            if(cur == p || cur == q) map.put(cur,cur);
            else{
                TreeNode l = map.getOrDefault(cur.left,null);
                TreeNode r = map.getOrDefault(cur.right,null);
                if(l == null && r == null) map.put(cur,null);
                else if(l != null && r != null) return cur;
                else map.put(cur, l == null? r:l);
            }
            
            
            prev = cur;
            cur = null;
        }
        return map.get(root);
       
    }
}
```