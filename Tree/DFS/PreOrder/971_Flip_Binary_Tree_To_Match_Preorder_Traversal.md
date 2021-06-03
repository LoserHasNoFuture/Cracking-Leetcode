# 971. Flip Binary Tree To Match Preorder Traversal
You are given the  `root`  of a binary tree with  `n`  nodes, where each node is uniquely assigned a value from  `1`  to  `n`. You are also given a sequence of  `n`  values  `voyage`, which is the  **desired**  [**pre-order traversal**](https://en.wikipedia.org/wiki/Tree_traversal#Pre-order)  of the binary tree.

Any node in the binary tree can be  **flipped**  by swapping its left and right subtrees. For example, flipping node 1 will have the following effect:

![](https://assets.leetcode.com/uploads/2021/02/15/fliptree.jpg)

Flip the  **smallest**  number of nodes so that the  **pre-order traversal**  of the tree  **matches**  `voyage`.

Return  _a list of the values of all  **flipped**  nodes. You may return the answer in  **any order**. If it is  **impossible**  to flip the nodes in the tree to make the pre-order traversal match_ `voyage`_, return the list_ `[-1]`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/01/02/1219-01.png)

**Input:** root = [1,2], voyage = [2,1]
**Output:** [-1]
**Explanation:** It is impossible to flip the nodes such that the pre-order traversal matches voyage.

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/01/02/1219-02.png)

**Input:** root = [1,2,3], voyage = [1,3,2]
**Output:** [1]
**Explanation:** Flipping node 1 swaps nodes 2 and 3, so the pre-order traversal matches voyage.

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/01/02/1219-02.png)

**Input:** root = [1,2,3], voyage = [1,2,3]
**Output:** []
**Explanation:** The tree's pre-order traversal already matches voyage, so no nodes need to be flipped.

**Constraints:**

-   The number of nodes in the tree is  `n`.
-   `n == voyage.length`
-   `1 <= n <= 100`
-   `1 <= Node.val, voyage[i] <= n`
-   All the values in the tree are  **unique**.
-   All the values in  `voyage`  are  **unique**.

# Solution 1: Recursive (Beat 92%)
```
class Solution {
    List<Integer> res = new ArrayList<Integer>();
    public List<Integer> flipMatchVoyage(TreeNode root, int[] voyage) {
        if(!dfs(root,voyage,0,voyage.length-1)){
            res = new ArrayList<Integer>();
            res.add(-1);
        }
        return res;
    }
    
    public boolean dfs(TreeNode root, int[] nums, int start, int end){
        if(root == null) return true;
        if(start == -1 || end < start || nums[start] != root.val) return false;
        int l = find_index(nums,start + 1, root.left);
        int r = find_index(nums,start + 1, root.right);
        if(l == -1 && r == -1) return true;
        if(l == -2 || r == -2 || (l != start + 1 && r != start + 1)) return false;
        if(l != -1 && l != start + 1){
            res.add(root.val);
            return dfs(root.right,nums,r,l-1) && dfs(root.left,nums,l,end);
        }else if(l == -1) return dfs(root.right,nums,r,end);
        else if(r == -1) return dfs(root.left,nums,l,end);
        else return dfs(root.right,nums,r,end) && dfs(root.left,nums,l,r-1);
        
    }
    
    public int find_index(int[] nums, int index, TreeNode root){
        if(root == null) return -1;
        for(int i = index; i < nums.length; i++){
            if(nums[i] == root.val) return i;
        }
        return -2;
    }
}
```


# Solution 2: Iterative (Beat 90%)
```
class Solution {

    public List<Integer> flipMatchVoyage(TreeNode root, int[] voyage) {
        List<Integer> res = new ArrayList<>();
        int i = 0;
        Stack<TreeNode> s = new Stack<>();
        s.push(root);
        while (s.size() > 0) {
            TreeNode node = s.pop();
            if (node == null) continue;
            if (node.val != voyage[i++]) return Arrays.asList(-1);
            if (node.right != null && node.right.val == voyage[i]) {
                if (node.left != null) res.add(node.val);
                s.push(node.left);
                s.push(node.right);
            } else {
                s.push(node.right);
                s.push(node.left);
            }
        }
        return res;
    }
}
```
