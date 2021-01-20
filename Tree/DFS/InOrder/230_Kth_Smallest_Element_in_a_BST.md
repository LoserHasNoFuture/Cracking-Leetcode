# 230. Kth Smallest Element in a BST
Given a binary search tree, write a function  `kthSmallest`  to find the  **k**th smallest element in it.

**Example 1:**

**Input:** root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
**Output:** 1

**Example 2:**

**Input:** root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
**Output:** 3

**Follow up:**  
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine? -->need extra space to save order info (number of nodes in left subtree)

**Constraints:**

-   The number of elements of the BST is between  `1`  to  `10^4`.
-   You may assume  `k`  is always valid,  `1 ≤ k ≤ BST's total elements`.

# Solution 1: DFS Recursion
```
class Solution {
    int res;
    public int kthSmallest(TreeNode root, int k) {
        dfs(root,k);
        return res;
    }
    
    public int dfs(TreeNode root, int k){
        if(root == null || k == 0) return k;
        k = dfs(root.left, k);
        k--;
        if(k == 0){
            res = root.val;
            return k;
        }
        return dfs(root.right,k);
    }
    
}
```

# Solution 2: Stack
```
class Solution {
    
    public int kthSmallest(TreeNode root, int k) {
        Stack<TreeNode> stack = new Stack();
        TreeNode cur = root;
        while(cur != null || !stack.isEmpty()){
            while(cur != null){
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.pop();
            k--;
            if(k==0) return cur.val;
            cur = cur.right;
        }
        
        return 0;
    }
    
}
```

# Solution 3: Morris Traversal
```
class Solution {
    
    public int kthSmallest(TreeNode root, int k) {
        TreeNode cur = root;
        while (cur != null){
            if(cur.left == null){
                k--;
                if(k == 0) return cur.val;
                cur = cur.right;
            }else{
                TreeNode prev = cur.left;
                while(prev.right!=null && prev.right !=cur) prev=prev.right;
                if(prev.right == null){
                    prev.right = cur;
                    cur = cur.left;
                }else{
                    prev.right = null;
                    k--;
                    if(k == 0) return cur.val;
                    cur = cur.right;
                }
            }
        }
        return 0;
    }
    
}
```