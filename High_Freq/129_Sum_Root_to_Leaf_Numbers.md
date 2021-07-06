# 129. Sum Root to Leaf Numbers
You are given the  `root`  of a binary tree containing digits from  `0`  to  `9`  only.

Each root-to-leaf path in the tree represents a number.

-   For example, the root-to-leaf path  `1 -> 2 -> 3`  represents the number  `123`.

Return  _the total sum of all root-to-leaf numbers_. Test cases are generated so that the answer will fit in a  **32-bit**  integer.

A  **leaf**  node is a node with no children.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/num1tree.jpg)

**Input:** root = [1,2,3]
**Output:** 25
**Explanation:**
The root-to-leaf path `1->2` represents the number `12`.
The root-to-leaf path `1->3` represents the number `13`.
Therefore, sum = 12 + 13 = `25`.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/num2tree.jpg)

**Input:** root = [4,9,0,5,1]
**Output:** 1026
**Explanation:**
The root-to-leaf path `4->9->5` represents the number 495.
The root-to-leaf path `4->9->1` represents the number 491.
The root-to-leaf path `4->0` represents the number 40.
Therefore, sum = 495 + 491 + 40 = `1026`.

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 1000]`.
-   `0 <= Node.val <= 9`
-   The depth of the tree will not exceed  `10`.

# Solution 1: Recursive DFS (Beat 100%)
```
class Solution {
    int res = 0;
    public int sumNumbers(TreeNode root) {
        if(root == null) return res;
        dfs(root, 0);
        return res;
    }
    
    public void dfs(TreeNode root, int sum){
        int cur = sum*10 + root.val;
        if(root.left == null && root.right == null){
            res += cur;
            return;
        }
        
        if(root.left != null) dfs(root.left, cur);
        if(root.right != null) dfs(root.right, cur);
    }
}
```

# Solution 2: Iterative PostOrder (Beat 26%)
```
class Solution {

    public int sumNumbers(TreeNode root) {
        int res = 0;
        if(root == null) return res;
        Stack<TreeNode> stack = new Stack();
        TreeNode cur = root;
        int path_sum = 0;
        TreeNode prev = null;
        while(!stack.isEmpty() || cur != null){
            while(cur != null){
                path_sum = path_sum* 10 + cur.val;
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.peek();
            if(cur.right != null && cur.right != prev){
                cur = cur.right;
                continue;
            }
            if(cur.left == null && cur.right == null){
                res += path_sum;
            }
            stack.pop();
            path_sum = (path_sum - cur.val)/10;
            prev = cur;
            cur = null;
        }
        
        return res;
    }
}
```