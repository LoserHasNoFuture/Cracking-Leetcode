# 173. Binary Search Tree Iterator
Implement the  `BSTIterator`  class that represents an iterator over the  **[in-order traversal](https://en.wikipedia.org/wiki/Tree_traversal#In-order_(LNR))**  of a binary search tree (BST):

-   `BSTIterator(TreeNode root)`  Initializes an object of the  `BSTIterator`  class. The  `root`  of the BST is given as part of the constructor. The pointer should be initialized to a non-existent number smaller than any element in the BST.
-   `boolean hasNext()`  Returns  `true`  if there exists a number in the traversal to the right of the pointer, otherwise returns  `false`.
-   `int next()`  Moves the pointer to the right, then returns the number at the pointer.

Notice that by initializing the pointer to a non-existent smallest number, the first call to  `next()`  will return the smallest element in the BST.

You may assume that  `next()`  calls will always be valid. That is, there will be at least a next number in the in-order traversal when  `next()`  is called.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/25/bst-tree.png)

**Input**
["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]
[[[7, 3, 15, null, null, 9, 20]], [], [], [], [], [], [], [], [], []]
**Output**
[null, 3, 7, true, 9, true, 15, true, 20, false]

**Explanation**
BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]);
bSTIterator.next();    // return 3
bSTIterator.next();    // return 7
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 9
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 15
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 20
bSTIterator.hasNext(); // return False

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 105]`.
-   `0 <= Node.val <= 106`
-   At most  `105`  calls will be made to  `hasNext`, and  `next`.

**Follow up:**

-   Could you implement  `next()`  and  `hasNext()`  to run in average  `O(1)`  time and use `O(h)`  memory, where  `h`  is the height of the tree?


# Solution 1: Inorder Traversal
```
class BSTIterator {

    TreeNode cur;
    Deque<TreeNode> stack;
    public BSTIterator(TreeNode root) {
        cur = root;
        stack = new ArrayDeque<TreeNode>();
    }
    
    public int next() {
        int res = -1;
        if(hasNext()){
            while(cur != null){
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.pop();
            res = cur.val;
            cur = cur.right;
        }
        return res;
    }
    
    public boolean hasNext() {
        return (cur != null || !stack.isEmpty());
    }
}

```

Better Writing:
```
class BSTIterator {

    Deque<TreeNode> stack;
    public BSTIterator(TreeNode root) {
        stack = new ArrayDeque<TreeNode>();
        push_all_to_left(root);
    }
    
    public int next() {
        int res = -1;
        if(hasNext()){
            TreeNode cur = stack.pop();
            res = cur.val;
            push_all_to_left(cur.right);
        }
        
        return res;
    }
    
    public boolean hasNext() {
        return !stack.isEmpty();
    }
    
    public void push_all_to_left(TreeNode root){
        while(root != null){
            stack.push(root);
            root = root.left;
        }
    }
}

```

Morris-traversal based:
```
class BSTIterator {

    TreeNode cur;
    public BSTIterator(TreeNode root) {
        cur = root;
    }
    
    /** @return the next smallest number */
    public int next() {
        int res = 0;        
        while(cur !=null){
            if(cur.left == null){
                res = cur.val;
                cur = cur.right;
                break;
            }else{
                TreeNode prev = cur.left;
                while(prev.right != null && prev.right != cur) prev = prev.right;
                
                if(prev.right == null){
                    prev.right = cur;
                    cur = cur.left;
                }else{
                    res = cur.val;
                    prev.right = null;
                    cur = cur.right;
                    break;
                }
            }
        }
        return res;
    }
    
    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return (cur != null);
    }
}
```