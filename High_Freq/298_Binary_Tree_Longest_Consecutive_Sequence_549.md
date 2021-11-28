# 298. Binary Tree Longest Consecutive Sequence 549
Given the  `root`  of a binary tree, return  _the length of the longest consecutive sequence path_.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path needs to be from parent to child (cannot be the reverse).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/consec1-1-tree.jpg)

**Input:** root = [1,null,3,2,4,null,null,null,5]
**Output:** 3
**Explanation:** Longest consecutive sequence path is 3-4-5, so return 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/consec1-2-tree.jpg)

**Input:** root = [2,null,3,2,null,1]
**Output:** 2
**Explanation:** Longest consecutive sequence path is 2-3, not 3-2-1, so return 2.

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 3 * 104]`.
-   `-3 * 104  <= Node.val <= 3 * 104`

# Solution: DFS + PostOrder (Beat 89%)
```
class Solution {
    
    int max = 0;
    public int longestConsecutive(TreeNode root) {
        if(root == null) return 0;
        dfs(root);
        return max;
    }
    
    public int dfs(TreeNode root){
        if(root == null) return 0;
        
        int l = dfs(root.left);
        int r = dfs(root.right);
        
        int len = 1;
        if(l != 0 && root.left.val == root.val + 1) len = Math.max(len,l+1);
        if(r != 0 && root.right.val == root.val + 1) len = Math.max(len,r+1);
        
        max = Math.max(len,max);
        return len;
    }
    
}
```