# 426. Convert Binary Search Tree to Sorted Doubly Linked List
Convert a  **Binary Search Tree** to a sorted  **Circular Doubly-Linked List** in place.

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

-   `-1000 <= Node.val <= 1000`
-   `Node.left.val < Node.val < Node.right.val`
-   All values of  `Node.val`  are unique.
-   `0 <= Number of Nodes <= 2000`

# Solution 1: Inorder Recursion (Tricky)
```
class Solution {
    Node head = null, last = null;
    public Node treeToDoublyList(Node root) {
        if(root == null) return null;
        dfs(root);
        head.left = last;
        last.right = head;
        return head;
    }
    
    public void dfs(Node root){
        if(root == null) return;
        dfs(root.left);
        if(head == null) head = root;
        if(last != null){
            last.right = root;
            root.left = last;
        }
        last = root;
        dfs(root.right);
    }
}
```

# Solution 2: Inorder Iteration (Stack)
```
class Solution {
    public Node treeToDoublyList(Node root) {
        if(root == null) return null;
            
        Node dummyHead = new Node(0);
        Node prev = dummyHead;
        Deque<Node> stack = new ArrayDeque<>();
        
        Node last = root;
        Node cur = root;
        while(!stack.isEmpty() || cur != null){
            while(cur != null){
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.pop();
            prev.right = cur;
            cur.left = prev;
            last = cur;
            
            prev = cur;
            cur = cur.right;
            
        }
        dummyHead.right.left = last;
        last.right = dummyHead.right;
        
        return dummyHead.right;
    }
```

# Solution 3: Inorder Morris Traversal
```
class Solution {
    public Node treeToDoublyList(Node root) {
        if(root == null) return null;
        Node dummyHead = new Node(0);
        Node prev = dummyHead;
        Node last = root;
        
        while(root != null){
            if(root.left == null){
                prev.right = root;
                root.left = prev;
                last = root;
                
                prev = prev.right;
                root = root.right;
            }else{
                Node temp = root.left;
                while(temp.right != null && temp.right != root) temp = temp.right;
                if(temp.right == null){
                    temp.right = root;
                    root = root.left;
                }else{
                    temp.right = null;
                    prev.right = root;
                    root.left = prev;
                    last = root;

                    prev = prev.right;
                    root = root.right;
                }
            }
        }
        
        dummyHead.right.left = last;
        last.right = dummyHead.right;
        
        return dummyHead.right;
    }
    
}
```