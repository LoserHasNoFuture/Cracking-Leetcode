# 1382. Balance a Binary Search Tree 108
Given a binary search tree, return a  **balanced**  binary search tree with the same node values.

A binary search tree is  _balanced_  if and only if the depth of the two subtrees of every node never differ by more than 1.

If there is more than one answer, return any of them.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2019/08/22/1515_ex1.png)![](https://assets.leetcode.com/uploads/2019/08/22/1515_ex1_out.png)**

**Input:** root = [1,null,2,null,3,null,4,null,null]
**Output:** [2,1,3,null,null,null,4]
**Explanation:** This is not the only correct answer, [3,1,4,null,2,null,null] is also correct.

**Constraints:**

-   The number of nodes in the tree is between `1` and `10^4`.
-   The tree nodes will have distinct values between `1` and `10^5`.

Unless using the idea of min heap, it is necessary to traverse the tree before starting constructing BST, as we do not the root of the tree beforeHand.

As inorder is extensively discussed, so I pick dfs here and only discuss how to converst sorted array to binary search tree.

# Solution: Recursively Construct BST
```
class Solution {
    ArrayList<Integer> list = new ArrayList<>();
    public TreeNode balanceBST(TreeNode root) {
        if(root == null) return root;
        dfs(root);        
        return constructBSTFromSortedList(0,list.size() - 1);
    }
    
    public void dfs(TreeNode root){
        if(root == null) return;
        dfs(root.left);
        list.add(root.val);
        dfs(root.right);
    }
    
    public TreeNode constructBSTFromSortedList(int start, int end){
        if(start > end) return null;
        if(start == end) return new TreeNode(list.get(start));
        int mid = (start + end) >>> 1;
        TreeNode root = new TreeNode(list.get(mid));
        root.left = constructBSTFromSortedList(start,mid-1);
        root.right = constructBSTFromSortedList(mid+1,end);
        
        return root;
    }
}
```

# Solution : Iteratively Construct BST
```
class Solution {
    ArrayList<Integer> nums = new ArrayList<>();
    public TreeNode balanceBST(TreeNode root) {
        if(root == null) return root;
        dfs(root);        
        return sortedArrayToBST();
    }
    
    public void dfs(TreeNode root){
        if(root == null) return;
        dfs(root.left);
        nums.add(root.val);
        dfs(root.right);
    }
    
    public TreeNode sortedArrayToBST() {
        
        int len = nums.size();
        if ( len == 0 ) { return null; }
        
        // 0 as a placeholder
        TreeNode head = new TreeNode(0); 
        
        Deque<TreeNode> nodeStack       = new LinkedList<TreeNode>() {{ push(head);  }};
        Deque<Integer>  leftIndexStack  = new LinkedList<Integer>()  {{ push(0);     }};
        Deque<Integer>  rightIndexStack = new LinkedList<Integer>()  {{ push(len-1); }};
        
        while ( !nodeStack.isEmpty() ) {
            TreeNode currNode = nodeStack.pop();
            int left  = leftIndexStack.pop();
            int right = rightIndexStack.pop();
            int mid   = left + (right-left)/2; // avoid overflow
            currNode.val = nums.get(mid);
            if ( left <= mid-1 ) {
                currNode.left = new TreeNode(0);  
                nodeStack.push(currNode.left);
                leftIndexStack.push(left);
                rightIndexStack.push(mid-1);
            }
            if ( mid+1 <= right ) {
                currNode.right = new TreeNode(0);
                nodeStack.push(currNode.right);
                leftIndexStack.push(mid+1);
                rightIndexStack.push(right);
            }
        }
        return head;
    }
}
```
