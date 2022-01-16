# 337. House Robber III
The thief has found himself a new place for his thievery again. There is only one entrance to this area, called  `root`.

Besides the  `root`, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if  **two directly-linked houses were broken into on the same night**.

Given the  `root`  of the binary tree, return  _the maximum amount of money the thief can rob  **without alerting the police**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg)

**Input:** root = [3,2,3,null,3,null,1]
**Output:** 7
**Explanation:** Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/10/rob2-tree.jpg)

**Input:** root = [3,4,5,1,3,null,1]
**Output:** 9
**Explanation:** Maximum amount of money the thief can rob = 4 + 5 = 9.

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 104]`.
-   `0 <= Node.val <= 104`

# Solution: Post order traversal
```
class Solution {
    class Pair{
        int root_sum;
        int child_sum;
        
        public Pair(int root, int child){
            this.root_sum = root;
            this.child_sum = child;
        }
    }
    
    public int rob(TreeNode root) {
        return dfs(root).root_sum;
    }
    
    public Pair dfs(TreeNode root){
        if(root == null) return new Pair(0,0);
        if(root.left == null && root.right == null) {
            return new Pair(root.val, 0);
        }
        
        Pair l = dfs(root.left);
        Pair r = dfs(root.right);
        
        int root_sum = root.val + l.child_sum + r.child_sum;
        int child_sum = l.root_sum + r.root_sum;
        root_sum = Math.max(root_sum, child_sum);
        
        return new Pair(root_sum, child_sum);
    }
}
```