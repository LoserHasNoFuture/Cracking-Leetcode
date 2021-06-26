# 199. Binary Tree Right Side View
Given the  `root`  of a binary tree, imagine yourself standing on the  **right side**  of it, return  _the values of the nodes you can see ordered from top to bottom_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/14/tree.jpg)

**Input:** root = [1,2,3,null,5,null,4]
**Output:** [1,3,4]

**Example 2:**

**Input:** root = [1,null,3]
**Output:** [1,3]

**Example 3:**

**Input:** root = []
**Output:** []

**Constraints:**

-   The number of nodes in the tree is in the range  `[0, 100]`.
-   `-100 <= Node.val <= 100`

# Solution 1: BFS
```
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if(root == null) return res;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while(!queue.isEmpty()){
            int sz = queue.size();
            
            for(int i = 0; i < sz; i++){
                TreeNode cur = queue.poll();
                if(i == sz - 1) res.add(cur.val);
                if(cur.left != null) queue.offer(cur.left);
                if(cur.right != null) queue.offer(cur.right);
            }
        }
        
        return res;
    }
}
```

# Solution 2: DFS Post-order
```
class Solution {
    List<Integer> res = new ArrayList<Integer>();
    // HashSet<Integer> set = new HashSet<Integer>();
    public List<Integer> rightSideView(TreeNode root) {
        dfs(root,0);
        return res;
    }
    
    public void dfs(TreeNode root, int depth){
        if(root == null) return;
        if(res.size() == depth){
            // set.add(depth);
            res.add(root.val);
        }
        dfs(root.right, depth+1);
        dfs(root.left, depth+1);
    }
}
```