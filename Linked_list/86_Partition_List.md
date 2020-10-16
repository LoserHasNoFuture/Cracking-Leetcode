# 86. Partition List
Given a linked list and a value  _x_, partition it such that all nodes less than  _x_  come before nodes greater than or equal to  _x_.

You should preserve the original relative order of the nodes in each of the two partitions.

**Example:**

**Input:** head = 1->4->3->2->5->2, _x_ = 3
**Output:** 1->2->2->4->3->5

# Solution
```
class Solution {
    public ListNode partition(ListNode head, int x) {
        if(head == null) return head;
        ListNode dummyHead1 = new ListNode(0);
        ListNode dummyHead2 = new ListNode(0);
        
        ListNode ptr1 = dummyHead1;
        ListNode ptr2 = dummyHead2;
        
        while(head != null){
            if(head.val < x){
                ptr1.next = head;
                ptr1 = ptr1.next;
            }else{
                ptr2.next = head;
                ptr2 = ptr2.next;
            }
            head = head.next;
        }
        
        ptr2.next = null;
        ptr1.next = dummyHead2.next;
        
        return dummyHead1.next;
    }
}
```