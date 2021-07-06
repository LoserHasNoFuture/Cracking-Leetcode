# 143. Reorder List
Given a singly linked list  _L_:  _L_0→_L_1→…→_L__n_-1→_L_n,  
reorder it to:  _L_0→_L__n_→_L_1→_L__n_-1→_L_2→_L__n_-2→…

You may  **not**  modify the values in the list's nodes, only nodes itself may be changed.

**Example 1:**

Given 1->2->3->4, reorder it to 1->4->2->3.

**Example 2:**

Given 1->2->3->4->5, reorder it to 1->5->2->4->3.

# Solution:
1. find middle point
2. reverse the second part
3. merge
```
class Solution {
    public void reorderList(ListNode head) {
        if(head == null || head.next == null) return;
        ListNode slow = head;
        ListNode fast = head.next;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode mid_head = slow.next;
        slow.next = null;
        
        mid_head = reverseList(mid_head);
        
        ListNode cur = new ListNode(0);
        while(head !=null && mid_head != null){
            cur.next = head; head = head.next;
            cur = cur.next;  
            cur.next = mid_head; mid_head = mid_head.next;
            cur = cur.next;
        }        
        if(head!=null) cur.next = head;
        
    }
    
    public ListNode reverseList(ListNode head){
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