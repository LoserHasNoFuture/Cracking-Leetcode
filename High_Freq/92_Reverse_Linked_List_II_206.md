# 92. Reverse Linked List II 206
Reverse a linked list from position  _m_  to  _n_. Do it in one-pass.

**Note:** 1 ≤  _m_  ≤  _n_  ≤ length of list.

**Example:**

**Input:** 1->2->3->4->5->NULL, _m_ = 2, _n_ = 4
**Output:** 1->4->3->2->5->NULL

# Solution:
```
class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        int change_len = n - m + 1;
        ListNode result = head;
        ListNode pre_head = null;
        while(--m > 0 && head != null){
            pre_head = head;
            head = head.next;
        }
        ListNode new_tail = head;
        ListNode new_head = null;
        
        while(head != null && change_len-- > 0){
            ListNode next = head.next;
            head.next = new_head;
            new_head = head; class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        int change_len = n - m + 1;
        ListNode result = head;
        ListNode pre_head = null;
        while(--m > 0 && head != null){
            pre_head = head;
            head = head.next;
        }
        ListNode new_tail = head;
        ListNode new_head = null;
        
        while(head != null && change_len-- > 0){
            ListNode next = head.next;
            head.next = new_head;
            new_head = head;
            head = next;
        }
        
        new_tail.next = head;
        
        if(pre_head != null) {
            pre_head.next = new_head;
        }else{
            result = new_head;
        }
        
        
        return result;
        
    }
}
            head = next;
        }
        
        new_tail.next = head;
        
        if(pre_head != null) {
            pre_head.next = new_head;
        }else{
            result = new_head;
        }
        
        return result;
    }
}
```