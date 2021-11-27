# 426. Convert Binary Search Tree to Sorted Doubly Linked List
Convert a  **Binary Search Tree**  to a sorted  **Circular Doubly-Linked List**  in place.

You can think of the left and right pointers as synonymous to the predecessor and successor pointers in a doubly-linked list. For a circular doubly linked list, the predecessor of the first element is the last element, and the successor of the last element is the first element.

We want to do the transformation  **in place**. After the transformation, the left pointer of the tree node should point to its predecessor, and the right pointer should point to its successor. You should return the pointer to the smallest element of the linked list.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/12/bstdlloriginalbst.png)

**Input:** root = [4,2,5,1,3]

![](https://assets.leetcode.com/uploads/2018/10/12/bstdllreturndll.png)
**Output:** [1,2,3,4,5]

**Explanation:** The figure below shows the transformed BST. The solid line indicates the successor relationship, while the dashed line means the predecessor relationship.
![](https://assets.leetcode.com/uploads/2018/10/12/bstdllreturnbst.png)

**Example 2:**

**Input:** root = [2,1,3]
**Output:** [1,2,3]

**Example 3:**

**Input:** root = []
**Output:** []
**Explanation:** Input is an empty tree. Output is also an empty Linked List.

**Example 4:**

**Input:** root = [1]
**Output:** [1]

**Constraints:**

-   The number of nodes in the tree is in the range  `[0, 2000]`.
-   `-1000 <= Node.val <= 1000`
-   All the values of the tree are  **unique**.


# Solution 1: DFS Recursion
```
class Solution {
    Node prev = null, head = null;
    public Node treeToDoublyList(Node root) {
        if(root == null) return root;
        dfs(root);
        head.left = prev;
        prev.right = head;
        return head;
    }
    
    public void dfs(Node root){
        if(root == null) return;
        dfs(root.left);
        if(prev != null) {
            prev.right = root;
            root.left = prev;
        }else head = root;
        prev = root;
        dfs(root.right);
    }
}
```

# Solution 2: DFS Iteration
```
class Solution {
    
    public Node treeToDoublyList(Node root) {
        if(root == null) return root;
        Node prev = null, head = null;
        Deque<Node> stack = new ArrayDeque<Node>();
        
        while(root != null || !stack.isEmpty()){
            while(root != null){
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            if(prev == null) head = root;
            else{
                prev.right = root;
                root.left = prev;
            }
            prev = root;
            root = root.right;
        }
        
        prev.right = head;
        head.left = prev;
        return head;
    }
}
```