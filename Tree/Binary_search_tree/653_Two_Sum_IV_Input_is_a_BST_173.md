# 653. Two Sum IV - Input is a BST 173
Given the  `root`  of a Binary Search Tree and a target number  `k`, return  _`true`  if there exist two elements in the BST such that their sum is equal to the given target_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/21/sum_tree_1.jpg)

**Input:** root = [5,3,6,2,4,null,7], k = 9
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/21/sum_tree_2.jpg)

**Input:** root = [5,3,6,2,4,null,7], k = 28
**Output:** false

**Example 3:**

**Input:** root = [2,1,3], k = 4
**Output:** true

**Example 4:**

**Input:** root = [2,1,3], k = 1
**Output:** false

**Example 5:**

**Input:** root = [2,1,3], k = 3
**Output:** true

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 104]`.
-   `-104 <= Node.val <= 104`
-   `root`  is guaranteed to be a  **valid**  binary search tree.
-   `-105 <= k <= 105`

# Solution: BST Iterator + Two Pointers
```
class Solution {
    public boolean findTarget(TreeNode root, int k) {
        BSTIterator left = new BSTIterator(root, true);
        BSTIterator right = new BSTIterator(root, false);
        
        TreeNode l = left.next(), r = right.next();
        while(l != null && r != null && l != r){            
            if(l.val + r.val == k) return true;
            if(l.val + r.val > k) r = right.next();
            else l = left.next();
        }
        
        return false;
    }
    
    class BSTIterator{
        
        boolean isIncreasing;
        Deque<TreeNode> stack = new ArrayDeque<TreeNode>();
        public BSTIterator(TreeNode root, boolean _isIncreasing){
            this.isIncreasing = _isIncreasing;
            if(isIncreasing) pushAllLeft(root);
            else pushAllRight(root);
        }
        
        public boolean hasNext(){
            return !stack.isEmpty();
        }
        
        public TreeNode next(){
            TreeNode res = null;
            
            if(hasNext()){
                TreeNode cur = stack.pop();
                res = cur;
                if(isIncreasing) {
                    cur = cur.right;
                    pushAllLeft(cur);
                }else{
                    cur = cur.left;
                    pushAllRight(cur);
                }
            }
            
            return res;
        }
        
        public void pushAllLeft(TreeNode root){
            while(root != null){
                stack.push(root);
                root = root.left;
            }
        }
        
        public void pushAllRight(TreeNode root){
            while(root != null){
                stack.push(root);
                root = root.right;
            }
        }
    }
}
```