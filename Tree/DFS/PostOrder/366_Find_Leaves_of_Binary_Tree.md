# 366. Find Leaves of Binary Tree
Given a binary tree, collect a tree's nodes as if you were doing this: Collect and remove all leaves, repeat until the tree is empty.

**Example:**

**Input:** [1,2,3,4,5] 1
         / \
        2   3
       / \     
      4   5    

**Output:** [[4,5,3],[2],[1]]

**Explanation:**

1. Removing the leaves  `[4,5,3]`  would result in this tree:

          1
         / 
        2          

2. Now removing the leaf  `[2]`  would result in this tree:

          1          

3. Now removing the leaf  `[1]`  would result in the empty tree:

          []         

[[3,5,4],[2],[1]], [[3,4,5],[2],[1]], etc, are also consider correct answers since per each level it doesn't matter the order on which elements are returned.

# Solution 1: DFS Recursion
```
class Solution {
    LinkedList<List<Integer>> res = new LinkedList<List<Integer>>();
    public List<List<Integer>> findLeaves(TreeNode root) {
        dfs(root);
        return res;
    }
    
    public int dfs(TreeNode root){
        if(root == null) return -1;
        int left = dfs(root.left) + 1;
        int right = dfs(root.right) + 1;
        int max = Math.max(left,right);
        if(res.size() > max) res.get(max).add(root.val);
        else {
            LinkedList<Integer> li = new LinkedList<Integer>();
            li.add(root.val);
            res.add(li);
        }
        return max;
    }
}
```

# Solution 2: DFS Iteration
```
class Solution {
    
    public List<List<Integer>> findLeaves(TreeNode root) {
        LinkedList<List<Integer>> res = new LinkedList<List<Integer>>();
        if(root == null) return res;
        HashMap<TreeNode,Integer> map = new HashMap<TreeNode,Integer>();
        Deque<TreeNode> stack = new ArrayDeque<>();
        stack.push(root);
        
        while(!stack.isEmpty()){
            if(stack.peek().left != null 
               && !map.containsKey(stack.peek().left)) stack.push(stack.peek().left);
            else if(stack.peek().right != null
                   && !map.containsKey(stack.peek().right)) stack.push(stack.peek().right);
            else{
                TreeNode cur = stack.pop();
                int left = map.getOrDefault(cur.left,-1) + 1;
                int right = map.getOrDefault(cur.right,-1) + 1;
                int max = Math.max(left,right);
                map.put(cur,max);
                if(res.size() > max) res.get(max).add(cur.val);
                else {
                    LinkedList<Integer> li = new LinkedList<Integer>();
                    li.add(cur.val);
                    res.add(li);
                }
            }
        }

        return res;
    }    
}
```
