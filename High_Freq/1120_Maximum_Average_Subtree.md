# 1120. Maximum Average Subtree
Given the  `root`  of a binary tree, return  _the maximum  **average**  value of a  **subtree**  of that tree_. Answers within  `10-5`  of the actual answer will be accepted.

A  **subtree**  of a tree is any node of that tree plus all its descendants.

The  **average**  value of a tree is the sum of its values, divided by the number of nodes.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/04/09/1308_example_1.png)

**Input:** root = [5,6,1]
**Output:** 6.00000
**Explanation:** 
For the node with value = 5 we have an average of (5 + 6 + 1) / 3 = 4.
For the node with value = 6 we have an average of 6 / 1 = 6.
For the node with value = 1 we have an average of 1 / 1 = 1.
So the answer is 6 which is the maximum.

**Example 2:**

**Input:** root = [0,null,1]
**Output:** 1.00000

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 104]`.
-   `0 <= Node.val <= 105`

# Solution:
```
class Solution {
    
    class Pair{
        int val;
        int cnt;
        
        public Pair(int _val, int _cnt){
            this.val = _val;
            this.cnt = _cnt;
        }
    }
    
    double res = Double.MIN_VALUE;
    public double maximumAverageSubtree(TreeNode root) {
        dfs(root);
        return res;
    }
    
    public Pair dfs(TreeNode root){
        if(root == null) return new Pair(0,0);
        if(root.left == null && root.right == null){
            res = Math.max(res,(double)root.val);
            return new Pair(root.val,1);
        }
        Pair left = dfs(root.left);
        Pair right = dfs(root.right);
        
        int cnt = left.cnt + right.cnt + 1;
        int val = left.val + right.val + root.val;
        res = Math.max(res,(double)val/cnt);
        
        return new Pair(val,cnt);
    }
}
```
