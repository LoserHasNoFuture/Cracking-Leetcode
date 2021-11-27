# 1382. Balance a Binary Search Tree
Given the  `root`  of a binary search tree, return  _a  **balanced**  binary search tree with the same node values_. If there is more than one answer, return  **any of them**.

A binary search tree is  **balanced**  if the depth of the two subtrees of every node never differs by more than  `1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/08/10/balance1-tree.jpg)

**Input:** root = [1,null,2,null,3,null,4,null,null]
**Output:** [2,1,3,null,null,null,4]
**Explanation:** This is not the only correct answer, [3,1,4,null,2] is also correct.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/08/10/balanced2-tree.jpg)

**Input:** root = [2,1,3]
**Output:** [2,1,3]

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 104]`.
-   `1 <= Node.val <= 105`

# Solution: build from sorted list
```
class Solution {
    
    List<Integer> arr;
    public TreeNode balanceBST(TreeNode root) {
        arr = new ArrayList<Integer>();
        dfs(root);
        
        int[] nums = new int[arr.size()];
        for(int i = 0; i < arr.size(); i++) nums[i] = arr.get(i);
        return sortedArrayToBST(nums);
    }
    
    public void dfs(TreeNode root){
        if(root == null) return;
        dfs(root.left);
        arr.add(root.val);
        dfs(root.right);
    }
    
    public TreeNode sortedArrayToBST(int[] nums) {
        if(nums.length == 0) return null;
        return buildTree(nums,  0, nums.length - 1);
    }
    
    public TreeNode buildTree(int[] nums, int start, int end){    
        if(start > end) return null;
        int mid = (start + end) >>> 1;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = buildTree(nums, start, mid-1);
        root.right = buildTree(nums, mid+1, end);
        return root;
    }
}
```