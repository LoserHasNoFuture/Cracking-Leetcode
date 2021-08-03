# 549. Binary Tree Longest Consecutive Sequence II 298
Given the  `root`  of a binary tree, return  _the length of the longest consecutive path in the tree_.

A consecutive path is a path where the values of the consecutive nodes in the path differ by one. This path can be either increasing or decreasing.

-   For example,  `[1,2,3,4]`  and  `[4,3,2,1]`  are both considered valid, but the path  `[1,2,4,3]`  is not valid.

On the other hand, the path can be in the child-Parent-child order, where not necessarily be parent-child order.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/consec2-1-tree.jpg)

**Input:** root = [1,2,3]
**Output:** 2
**Explanation:** The longest consecutive path is [1, 2] or [2, 1].

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/consec2-2-tree.jpg)

**Input:** root = [2,1,3]
**Output:** 3
**Explanation:** The longest consecutive path is [1, 2, 3] or [3, 2, 1].

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 3 * 104]`.
-   `-3 * 104  <= Node.val <= 3 * 104`

# Solution 1: DFS + PostOrder (Beat 100%)
```
class Solution {
    int max = 0;
    public int longestConsecutive(TreeNode root) {
        if(root == null) return max;
        dfs(root);
        return max;
    }
    
    class Path{
        int pLen, nLen;
        
        public Path(int _pLen, int _nLen){
            this.pLen = _pLen; // root is larger than children
            this.nLen =_nLen; // root is smaller than children
        }
    }
    
    public Path dfs(TreeNode root){
        if(root == null) return null;
        if(root.left == null && root.right == null) {
            max = Math.max(1,max);
            return new Path(1,1);
        }
        
        Path left = dfs(root.left);
        Path right = dfs(root.right);
        
        int pLen = 1, nLen = 1;
        if(left != null){
            if(root.val - root.left.val == 1){
                if(right != null && root.val - root.right.val == -1) {
                    max = Math.max(max, left.pLen+1 + right.nLen);
                }
                pLen = Math.max(pLen, left.pLen+1);
            }else if (root.val - root.left.val == -1){
                if(right != null && root.val - root.right.val == 1) {
                    max = Math.max(max, left.nLen+1 + right.pLen);
                }
                nLen = Math.max(nLen, left.nLen+1);
            }   
        }
        
        if(right != null){
            if(root.val - root.right.val == 1){
                pLen = Math.max(pLen, right.pLen+1);
            }else if (root.val - root.right.val == -1){
                nLen = Math.max(nLen, right.nLen+1);
            }   
        }
        
        max = Math.max(Math.max(max, pLen),nLen);
        
        return new Path(pLen, nLen);
    }
    
    
}
```