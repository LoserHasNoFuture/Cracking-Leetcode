# 333. Largest BST Subtree
Given the root of a binary tree, find the largest subtree, which is also a Binary Search Tree (BST), where the largest means subtree has the largest number of nodes.

A  **Binary Search Tree (BST)**  is a tree in which all the nodes follow the below-mentioned properties:

-   The left subtree values are less than the value of their parent (root) node's value.
-   The right subtree values are greater than the value of their parent (root) node's value.

**Note:** A subtree must include all of its descendants.

**Follow up:** Can you figure out ways to solve it with O(n) time complexity?

**Example 1:**

**![](https://assets.leetcode.com/uploads/2020/10/17/tmp.jpg)**

**Input:** root = [10,5,15,1,8,null,7]
**Output:** 3
**Explanation:** The Largest BST Subtree in this case is the highlighted one. The return value is the subtree's size, which is 3.

**Example 2:**

**Input:** root = [4,2,7,2,3,5,null,2,null,null,null,null,null,1]
**Output:** 2

**Constraints:**

-   The number of nodes in the tree is in the range  `[0, 104]`.
-   `-104  <= Node.val <= 104`


# Solution: PostOrder Traversal (Beat 100%)
```
class Solution {
    
    class SubTreeInfo{
        int cnt = 0;
        boolean isBst = false;
        int max = 0;
        int min = 0;
        
        SubTreeInfo(){}
        
        SubTreeInfo(int _cnt, boolean _isBst, int _max, int _min){
            this.cnt = _cnt;
            this.isBst = _isBst;
            this.max = _max;
            this.min = _min;
        }
    }
    
    int max = 0;
    public int largestBSTSubtree(TreeNode root) {
        dfs(root);
        return max;
    }
    
    public SubTreeInfo dfs(TreeNode root){
        if(root == null) return new SubTreeInfo(0,true,Integer.MIN_VALUE, Integer.MAX_VALUE);
        
        SubTreeInfo l = dfs(root.left);
        SubTreeInfo r = dfs(root.right);
        
        if(l.isBst && r.isBst){
            if(l.max < root.val && r.min > root.val){
                if(l.cnt + r.cnt + 1 > max) max = l.cnt + r.cnt + 1;
                return new SubTreeInfo(l.cnt + r.cnt + 1,true, Math.max(root.val,r.max), Math.min(root.val,l.min));
            }
        }
        
        return new SubTreeInfo();
    }
}
```