# 366. Find Leaves of Binary Tree
Given the  `root`  of a binary tree, collect a tree's nodes as if you were doing this:

-   Collect all the leaf nodes.
-   Remove all the leaf nodes.
-   Repeat until the tree is empty.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/16/remleaves-tree.jpg)

**Input:** root = [1,2,3,4,5]
**Output:** [[4,5,3],[2],[1]]
Explanation:
[[3,5,4],[2],[1]] and [[3,4,5],[2],[1]] are also considered correct answers since per each level it does not matter the order on which elements are returned.

**Example 2:**

**Input:** root = [1]
**Output:** [[1]]

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 100]`.
-   `-100 <= Node.val <= 100`


# Solution: Post-Order Traversal
```
class Solution {
    List<List<Integer>> res;
    public List<List<Integer>> findLeaves(TreeNode root) {
        res = new ArrayList<List<Integer>>();
        dfs(root);
        return res;
    }
    
    public int dfs(TreeNode root){
        if(root == null) return -1;
        int left = dfs(root.left);
        int right = dfs(root.right);
        int cur_index = Math.max(left, right)+1;
        if(cur_index == res.size()) res.add(new ArrayList<Integer>());
        res.get(cur_index).add(root.val);
        return cur_index;
    }
}
```

# Solution: Post-Order Iterative
```
class Solution {
    
    public List<List<Integer>> findLeaves(TreeNode root) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if(root == null) return res;
        Deque<TreeNode> stack = new ArrayDeque();
        HashMap<TreeNode, Integer> map = new HashMap<TreeNode, Integer>();
        
        TreeNode cur = root;
        TreeNode prev = null;
        while(!stack.isEmpty() || cur != null){
            if(cur != null){
                stack.push(cur);
                cur = cur.left;
            }else{
                cur = stack.peek();
                if(cur.right != null && cur.right != prev)
                    cur = cur.right;
                else{
                    stack.pop();
                    int l = map.getOrDefault(cur.left, -1);
                    int r = map.getOrDefault(cur.right, -1);
                    int depth = Math.max(l, r) + 1;
                    if(depth == res.size()) res.add(new ArrayList<Integer>());
                    res.get(depth).add(cur.val);
                    map.put(cur, depth);
                    prev = cur;
                    cur = null;        
                }
            }
        }
      
        return res;    
    }
    
}
```