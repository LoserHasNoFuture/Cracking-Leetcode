# 95. Unique Binary Search Trees II 96
Given an integer  `n`, return  _all the structurally unique  **BST'**s (binary search trees), which has exactly_ `n` _nodes of unique values from_  `1`  _to_  `n`. Return the answer in  **any order**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

**Input:** n = 3
**Output:** [[1,null,2,null,3],[1,null,3,2],[2,1,3],[3,1,null,null,2],[3,2,null,1]]

**Example 2:**

**Input:** n = 1
**Output:** [[1]]

**Constraints:**

-   `1 <= n <= 8`

# Solution: Back Tracking
```
class Solution {
    public List<TreeNode> generateTrees(int n) {
        return dfs(1,n);
    }
    
    public List<TreeNode> dfs(int start, int end){
        List<TreeNode> res = new ArrayList<TreeNode>();
        if(start > end) {
            res.add(null);
            return res;
        }
        if(start == end) {
            res.add(new TreeNode(start));
            return res;
        }
        
        for(int i = start; i <= end; i++){
            List<TreeNode> left = dfs(start, i-1);
            List<TreeNode> right = dfs(i+1, end);
            
            for(TreeNode l : left){
                for(TreeNode r: right){
                    TreeNode root = new TreeNode(i);
                    root.left = l;
                    root.right = r;
                    res.add(root);
                }
            }
            
        }
        
        return res;
    }
}
```