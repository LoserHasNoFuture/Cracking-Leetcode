# 109. Convert Sorted List to Binary Search Tree
Given the  `head`  of a singly linked list where elements are  **sorted in ascending order**, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of  _every_  node never differ by more than 1.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/17/linked.jpg)

**Input:** head = [-10,-3,0,5,9]
**Output:** [0,-3,9,-10,null,5]
**Explanation:** One possible answer is [0,-3,9,-10,null,5], which represents the shown height balanced BST.

**Example 2:**

**Input:** head = []
**Output:** []

**Example 3:**

**Input:** head = [0]
**Output:** [0]

**Example 4:**

**Input:** head = [1,3]
**Output:** [3,1]

**Constraints:**

-   The number of nodes in  `head`  is in the range  `[0, 2 * 104]`.
-   `-10^5 <= Node.val <= 10^5`

# Solution: Divide and Conquer (Recursion)
```
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        if(head == null) return null;
        if(head.next == null) return new TreeNode(head.val);
        
//         find mid
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        ListNode prev = dummyHead;
        ListNode slow = head;
        ListNode fast = head;
        while(fast != null && fast.next != null){
            fast = fast.next.next;
            prev = slow;
            slow = slow.next;
        }
        
        TreeNode root = new TreeNode(slow.val);
        prev.next = null;
        root.left = sortedListToBST(head);
        root.right = sortedListToBST(slow.next);
        
        return root;
    }
}
```