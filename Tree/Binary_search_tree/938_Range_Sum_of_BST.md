# 938. Range Sum of BST
Given the  `root`  node of a binary search tree and two integers  `low`  and  `high`, return  _the sum of values of all nodes with a value in the  **inclusive**  range_ `[low, high]`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/05/bst1.jpg)

**Input:** root = [10,5,15,3,7,null,18], low = 7, high = 15
**Output:** 32
**Explanation:** Nodes 7, 10, and 15 are in the range [7, 15]. 7 + 10 + 15 = 32.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/05/bst2.jpg)

**Input:** root = [10,5,15,3,7,13,18,1,null,6], low = 6, high = 10
**Output:** 23
**Explanation:** Nodes 6, 7, and 10 are in the range [6, 10]. 6 + 7 + 10 = 23.

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 2 * 104]`.
-   `1 <= Node.val <= 105`
-   `1 <= low <= high <= 105`
-   All  `Node.val`  are  **unique**.

# Solution: Inorder
```
class Solution {
    int res = 0;
    public int rangeSumBST(TreeNode root, int low, int high) {
        dfs(root, low, high);
        return res;
    }
    
    public void dfs(TreeNode root, int low, int high){
        if(root == null) return;
        if(root.val < low) dfs(root.right, low, high);
        else if(root.val > high) dfs(root.left, low, high);
        else{
            dfs(root.left, low, high);
            res += root.val;
            dfs(root.right, low, high);
        }
    }
}
```