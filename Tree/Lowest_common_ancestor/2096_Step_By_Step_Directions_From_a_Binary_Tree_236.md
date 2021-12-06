# 2096. Step-By-Step Directions From a Binary Tree Node to Another 236
You are given the  `root`  of a  **binary tree**  with  `n`  nodes. Each node is uniquely assigned a value from  `1`  to  `n`. You are also given an integer  `startValue`  representing the value of the start node  `s`, and a different integer  `destValue`  representing the value of the destination node  `t`.

Find the  **shortest path**  starting from node  `s`  and ending at node  `t`. Generate step-by-step directions of such path as a string consisting of only the  **uppercase**  letters  `'L'`,  `'R'`, and  `'U'`. Each letter indicates a specific direction:

-   `'L'`  means to go from a node to its  **left child**  node.
-   `'R'`  means to go from a node to its  **right child**  node.
-   `'U'`  means to go from a node to its  **parent**  node.

Return  _the step-by-step directions of the  **shortest path**  from node_ `s` _to node_  `t`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/11/15/eg1.png)

**Input:** root = [5,1,2,3,null,6,4], startValue = 3, destValue = 6
**Output:** "UURL"
**Explanation:** The shortest path is: 3 → 1 → 5 → 2 → 6.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/11/15/eg2.png)

**Input:** root = [2,1], startValue = 2, destValue = 1
**Output:** "L"
**Explanation:** The shortest path is: 2 → 1.

**Constraints:**

-   The number of nodes in the tree is  `n`.
-   `2 <= n <= 105`
-   `1 <= Node.val <= n`
-   All the values in the tree are  **unique**.
-   `1 <= startValue, destValue <= n`
-   `startValue != destValue`

# Solution
```
class Solution {
    boolean find = false;
    public String getDirections(TreeNode root, int startValue, int destValue) {
        TreeNode LCA = lowestCommonAncestor(root, startValue, destValue);
        
        
        StringBuilder path_to_start = new StringBuilder();
        StringBuilder path_to_end = new StringBuilder();
        
        find_path(LCA, startValue, path_to_start);
        find = false;
        find_path(LCA, destValue, path_to_end);
        
        int len = path_to_start.length();
        path_to_start  =  new StringBuilder();
        while(--len >= 0) path_to_start.append("U");
        
        path_to_start.append(path_to_end.toString());
        return path_to_start.toString();
    }
    
    public void find_path(TreeNode root, int val, StringBuilder sb){
        if(find || root == null) return;
        if(root.val == val) {
            find = true;
            return;
        }
        
        sb.append("L");
        find_path(root.left, val, sb);
        if(find) return;
        sb.deleteCharAt(sb.length()-1);
        
        sb.append("R");
        find_path(root.right, val, sb);
        if(find) return;
        sb.deleteCharAt(sb.length()-1);
    }
    
    
    public TreeNode lowestCommonAncestor(TreeNode root, int p, int q) {
        if(root == null || root.val == p || root.val == q) return root;
        
        TreeNode l = lowestCommonAncestor(root.left, p, q);
        TreeNode r = lowestCommonAncestor(root.right, p, q);
        
        if(l == null && r == null) return null;
        if(l != null && r != null) return root;
        
        return l == null? r: l;
    }
}
```