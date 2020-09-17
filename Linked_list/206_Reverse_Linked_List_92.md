# 206. Reverse Linked List
Reverse a singly linked list.

**Example:**

**Input:** 1->2->3->4->5->NULL
**Output:** 5->4->3->2->1->NULL

**Follow up:**

A linked list can be reversed either iteratively or recursively. Could you implement both?

# Solution:
```
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode new_head = new ListNode(0);
        
        while(head != null){
            ListNode temp = head;
            head = head.next;
            temp.next = new_head.next;
            new_head.next = temp;
        }
        
        return new_head.next;
    }
}
```