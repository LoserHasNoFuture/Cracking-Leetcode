# 510. Inorder Successor in BST II  285
Given a  `node`  in a binary search tree, return  _the in-order successor of that node in the BST_. If that node has no in-order successor, return  `null`.

The successor of a  `node`  is the node with the smallest key greater than  `node.val`.

You will have direct access to the node but not to the root of the tree. Each node will have a reference to its parent node. Below is the definition for  `Node`:

class Node {
    public int val;
    public Node left;
    public Node right;
    public Node parent;
}

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/01/23/285_example_1.PNG)

**Input:** tree = [2,1,3], node = 1
**Output:** 2
**Explanation:** 1's in-order successor node is 2. Note that both the node and the return value is of Node type.

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/01/23/285_example_2.PNG)

**Input:** tree = [5,3,6,2,4,null,null,1], node = 6
**Output:** null
**Explanation:** There is no in-order successor of the current node, so the answer is null.

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/02/02/285_example_34.PNG)

**Input:** tree = [15,6,18,3,7,17,20,2,4,null,13,null,null,null,null,null,null,null,null,9], node = 15
**Output:** 17

**Example 4:**

![](https://assets.leetcode.com/uploads/2019/02/02/285_example_34.PNG)

**Input:** tree = [15,6,18,3,7,17,20,2,4,null,13,null,null,null,null,null,null,null,null,9], node = 13
**Output:** 15

**Example 5:**

**Input:** tree = [0], node = 0
**Output:** null

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 104]`.
-   `-105  <= Node.val <= 105`
-   All Nodes will have unique values.

**Follow up:**  Could you solve it without looking up any of the node's values?


# Solution 1: Inorder Traversal 
Recursive:
```
class Solution {
    public Node inorderSuccessor(Node node) {
        if(node.right == null) {
            return find_suc_from_parents(node);
        }else {
            Node cur = node.right;
            while(cur.left != null) cur = cur.left;
            return cur;
        }
    }
    
    public Node find_suc_from_parents(Node node){
        if(node.parent == null) return null;
        // right child of its parent
        if(node.parent.left != null && node.parent.left == node) return node.parent;
        // left child  of its parent
        else return find_suc_from_parents(node.parent);
    }
```


Iterative:
```
class Solution {
    public Node inorderSuccessor(Node node) {
        if(node.right == null) {
            Node cur = node;            
            while(cur.parent != null){
                if(cur.parent.left != null && cur.parent.left == cur) return cur.parent;
                else cur = cur.parent;
            }
            
            return null;
        }else {
            Node cur = node.right;
            while(cur.left != null) cur = cur.left;
            return cur;
        }
    }    
}
```