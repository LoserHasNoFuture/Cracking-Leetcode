# 270. Closest Binary Search Tree Value
Given the  `root`  of a binary search tree and a  `target`  value, return  _the value in the BST that is closest to the_  `target`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/12/closest1-1-tree.jpg)

**Input:** root = [4,2,5,1,3], target = 3.714286
**Output:** 4

**Example 2:**

**Input:** root = [1], target = 4.428571
**Output:** 1

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 104]`.
-   `0 <= Node.val <= 109`
-   `-109  <= target <= 109`

# Solution
```
class Solution {
    public int closestValue(TreeNode root, double target) {
        int res = Integer.MAX_VALUE;
        double diff = Double.MAX_VALUE; 
        
        while(root != null){
            if(root.val == target) return root.val;
            if(Math.abs(root.val - target) < diff){
                diff = Math.abs(root.val - target);
                res = root.val;
            }
            if(root.val > target) root = root.left;
            else root = root.right;
                
        }
        
        return res;
    }
}
```
