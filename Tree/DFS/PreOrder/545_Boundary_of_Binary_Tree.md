# 545. Boundary of Binary Tree
The  **boundary**  of a binary tree is the concatenation of the  **root**, the  **left boundary**, the  **leaves**  ordered from left-to-right, and the  **reverse order**  of the  **right boundary**.

The  **left boundary**  is the set of nodes defined by the following:

-   The root node's left child is in the left boundary. If the root does not have a left child, then the left boundary is  **empty**.
-   If a node in the left boundary and has a left child, then the left child is in the left boundary.
-   If a node is in the left boundary, has  **no**  left child, but has a right child, then the right child is in the left boundary.
-   The leftmost leaf is  **not**  in the left boundary.

The  **right boundary**  is similar to the  **left boundary**, except it is the right side of the root's right subtree. Again, the leaf is  **not**  part of the  **right boundary**, and the  **right boundary**  is empty if the root does not have a right child.

The  **leaves**  are nodes that do not have any children. For this problem, the root is  **not**  a leaf.

Given the  `root`  of a binary tree, return  _the values of its  **boundary**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/11/boundary1.jpg)

**Input:** root = [1,null,2,3,4]
**Output:** [1,3,4,2]
**Explanation:**
- The left boundary is empty because the root does not have a left child.
- The right boundary follows the path starting from the root's right child 2 -> 4.
  4 is a leaf, so the right boundary is [2].
- The leaves from left to right are [3,4].
Concatenating everything results in [1] + [] + [3,4] + [2] = [1,3,4,2].

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/11/boundary2.jpg)

**Input:** root = [1,2,3,4,5,6,null,null,null,7,8,9,10]
**Output:** [1,2,4,7,8,9,10,6,3]
**Explanation:**
- The left boundary follows the path starting from the root's left child 2 -> 4.
  4 is a leaf, so the left boundary is [2].
- The right boundary follows the path starting from the root's right child 3 -> 6 -> 10.
  10 is a leaf, so the right boundary is [3,6], and in reverse order is [6,3].
- The leaves from left to right are [4,7,8,9,10].
Concatenating everything results in [1] + [2] + [4,7,8,9,10] + [6,3] = [1,2,4,7,8,9,10,6,3].

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 104]`.
-   `-1000 <= Node.val <= 1000`

# Solution 1: Straight Forward (Beat 60%)
```
class Solution {
    List<Integer> leaves = new ArrayList<Integer>();
    public List<Integer> boundaryOfBinaryTree(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if(root == null) return res;
//         1. root
        res.add(root.val);
//         2. left boundray
        res.addAll(get_left_boundray(root.left));
//         3. leaves from left to right
        if(root.left != null || root.right != null){
            get_leaves(root);
            res.addAll(leaves);
        }
//         4. reverse order of right boundary
        res.addAll(get_right_boundray(root.right));
        return res;
    }
    
    public void get_leaves(TreeNode root){
        if(root == null) return;
        if(root.left == null && root.right == null) 
            leaves.add(root.val);
        get_leaves(root.left);
        get_leaves(root.right);
    }
    
    public List<Integer> get_left_boundray(TreeNode root){
        List<Integer> res = new ArrayList<Integer>();
        if(root == null) return res;
        
        while(root.left != null || root.right != null){
            res.add(root.val);
            if(root.left != null) root = root.left;
            else root = root.right;
        }
        
        return res;
    }
    
    public List<Integer> get_right_boundray(TreeNode root){
        List<Integer> res = new ArrayList<Integer>();
        if(root == null) return res;
        
        while(root.left != null || root.right != null){
            res.add(0,root.val);
            if(root.right != null) root = root.right;
            else root = root.left;
        }
        
        return res;
    }
}
```

# Solution 2: Modified One Pass Pre-order Traversal (Beat 100%)
Refer from: [https://leetcode.com/problems/boundary-of-binary-tree/discuss/101294/Java-C%2B%2B-Clean-Code-(1-Pass-perorder-postorder-hybrid)](https://leetcode.com/problems/boundary-of-binary-tree/discuss/101294/Java-C%2B%2B-Clean-Code-(1-Pass-perorder-postorder-hybrid))
1.  `node.left`  is  `left bound`  if  `node`  is left bound;  
    `node.right`  could also be left bound if  `node`  is left bound &&  `node`  has no right child;
2.  Same applys for  `right bound`;
3.  if node is  `left bound`, add it  `before`  2 child - pre order;  
    if node is  `right bound`, add it  `after`  2 child - post order;
4.  A  `leaf node`  that is neither left or right bound belongs to the bottom line;
```
public class Solution {
    
    public List<Integer> boundaryOfBinaryTree(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if (root != null) {
            res.add(root.val);
            getBounds(root.left, res, true, false);
            getBounds(root.right, res, false, true);
        }
        return res;
    }

    private void getBounds(TreeNode node, List<Integer> res, boolean lb, boolean rb) {
        if (node == null) return;
        if (lb) res.add(node.val);
        if (!lb && !rb && node.left == null && node.right == null) res.add(node.val);
        getBounds(node.left, res, lb, rb && node.right == null);
        getBounds(node.right, res, lb && node.left == null, rb);
        if (rb) res.add(node.val);
    }
}
```