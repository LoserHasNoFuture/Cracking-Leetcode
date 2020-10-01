# 24. Swap Nodes in Pairs 143 328 61
Given a linked list, swap every two adjacent nodes and return its head.

You may  **not**  modify the values in the list's nodes, only nodes itself may be changed.

**Example:**

Given `1->2->3->4`, you should return the list as `2->1->4->3`.

# Solution:
```
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        
        ListNode prev = dummyHead;
        ListNode ptr = head;
        
        while(ptr != null && ptr.next != null){
            prev.next = ptr.next;
            ListNode temp = ptr.next.next;
            ptr.next.next = ptr;
            ptr.next = temp;
            
            prev = ptr;
            ptr = ptr.next;
        }
         
        return dummyHead.next;
    }
}
```