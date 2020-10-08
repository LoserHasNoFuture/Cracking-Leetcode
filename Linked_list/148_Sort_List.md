# 148. Sort List
Given the  `head`  of a linked list, return  _the list after sorting it in  **ascending order**_.

**Follow up:**  Can you sort the linked list in  `O(n logn)`  time and  `O(1)` memory (i.e. constant space)?

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/14/sort_list_1.jpg)

**Input:** head = [4,2,1,3]
**Output:** [1,2,3,4]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/14/sort_list_2.jpg)

**Input:** head = [-1,5,3,4,0]
**Output:** [-1,0,3,4,5]

**Example 3:**

**Input:** head = []
**Output:** []

**Constraints:**

-   The number of nodes in the list is in the range  `[0, 5 * 104]`.
-   `-105  <= Node.val <= 105`


# Solution: Merge Sort
```
class Solution {
    public ListNode sortList(ListNode head) {
        if(head == null || head.next == null) return head;
        
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
        prev.next = null;
        
        head = sortList(head);
        slow = sortList(slow);
        
        
//         merge
        ListNode ptr = dummyHead;
        while(slow != null && head != null){
            if(slow.val < head.val){
                ptr.next = slow;
                slow = slow.next;
            }else{
                ptr.next = head;
                head = head.next;
            }
            ptr = ptr.next;
        }
        
        if(slow == null) ptr.next = head;
        if(head == null) ptr.next = slow;
        
        return dummyHead.next;
    }
    
}
```