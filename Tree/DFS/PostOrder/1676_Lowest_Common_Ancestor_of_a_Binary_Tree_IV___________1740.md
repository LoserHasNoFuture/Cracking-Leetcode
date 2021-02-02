# 1676. Lowest Common Ancestor of a Binary Tree IV
Given the  `root`  of a binary tree and an array of  `TreeNode`  objects  `nodes`, return  _the lowest common ancestor (LCA) of  **all the nodes**  in_ `nodes`. All the nodes will exist in the tree, and all values of the tree's nodes are  **unique**.

Extending the  **[definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor)**: "The lowest common ancestor of  `n`  nodes  `p1`,  `p2`, ...,  `pn`  in a binary tree  `T`  is the lowest node that has every  `pi`  as a  **descendant**  (where we allow  **a node to be a descendant of itself**) for every valid  `i`". A  **descendant**  of a node  `x`  is a node  `y`  that is on the path from node  `x`  to some leaf node.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Input:** root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [4,7]
**Output:** 2
**Explanation:** The lowest common ancestor of nodes 4 and 7 is node 2.

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Input:** root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [1]
**Output:** 1
**Explanation:** The lowest common ancestor of a single node is the node itself.

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Input:** root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [7,6,2,4]
**Output:** 5
**Explanation:** The lowest common ancestor of the nodes 7, 6, 2, and 4 is node 5.

**Example 4:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Input:** root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [0,1,2,3,4,5,6,7,8]
**Output:** 3
**Explanation:** The lowest common ancestor of all the nodes is the root node.

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 104]`.
-   `-109  <= Node.val <= 109`
-   All  `Node.val`  are  **unique**.
-   All  `nodes[i]`  will exist in the tree.
-   All  `nodes[i]`  are distinct.

# Solution 1: DFS (Beat 31%)
```
class Solution {
    int visited = 0;
    TreeNode res = null;
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode[] nodes) {
        HashSet<TreeNode> set = new HashSet<TreeNode>();
        visited = nodes.length;
        for(TreeNode node:nodes) set.add(node);
        dfs(root,set);
        return res;
    }
    
    
    public boolean dfs(TreeNode root, HashSet<TreeNode> set){
        if(root == null || visited == 0) return false;
        
        boolean left = false, right = false;
        if(dfs(root.left, set)) left = true;
        if(dfs(root.right, set)) right = true;
        
        if(set.contains(root)) {
            visited--;
            if(visited == 0) res = root;
            return true;
        }
        if(left && right) res = root;
        
        return left || right;
    }
}
```

# Solution 2: DFS (Optimized Version, Beat 90%)
Using List instead of HashSet, could beat 100%

```
class Solution { 
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode[] nodes) {
        HashSet<TreeNode> set = new HashSet<TreeNode>();
        for(TreeNode node:nodes) set.add(node);
        return dfs(root,set);
    }
    
    
    public TreeNode dfs(TreeNode root, HashSet<TreeNode> set){
        if(root == null || set.contains(root)) return root;
        
        TreeNode left = dfs(root.left, set);
        TreeNode right = dfs(root.right, set);
        if(left != null && right != null) return root;
        if(left != null) return left;
        else return right;
    }
}
```