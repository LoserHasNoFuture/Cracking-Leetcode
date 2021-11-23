# 103. Binary Tree Zigzag Level Order Traversal
Given the  `root`  of a binary tree, return  _the zigzag level order traversal of its nodes' values_. (i.e., from left to right, then right to left for the next level and alternate between).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

**Input:** root = [3,9,20,null,null,15,7]
**Output:** [[3],[20,9],[15,7]]

**Example 2:**

**Input:** root = [1]
**Output:** [[1]]

**Example 3:**

**Input:** root = []
**Output:** []

**Constraints:**

-   The number of nodes in the tree is in the range  `[0, 2000]`.
-   `-100 <= Node.val <= 100`

# Solution 1: BFS
```
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        Deque<TreeNode> queue = new ArrayDeque<TreeNode>();
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        
        if(root!=null) queue.offer(root);
        while(!queue.isEmpty()){
            int sz = queue.size();
            List<Integer> tmp = new ArrayList<Integer>();
            
            for(int i = 0; i < sz; i++){
                TreeNode cur = queue.poll();
                if(res.size()%2 == 0) tmp.add(cur.val);
                else tmp.add(0,cur.val);
                
                if(cur.left != null) queue.offer(cur.left);
                if(cur.right != null) queue.offer(cur.right);
            }
            
            res.add(tmp);
        }
        
        return res;
    }
}
```