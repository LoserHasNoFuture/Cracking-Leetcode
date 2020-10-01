# 328. Odd Even Linked List 61 24 143
Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.

**Example 1:**

**Input:** `1->2->3->4->5->NULL`
**Output:** `1->3->5->2->4->NULL`

**Example 2:**

**Input:** 2`->1->3->5->6->4->7->NULL`
**Output:** `2->3->6->7->1->5->4->NULL`

**Constraints:**

-   The relative order inside both the even and odd groups should remain as it was in the input.
-   The first node is considered odd, the second node even and so on ...
-   The length of the linked list is between  `[0, 10^4]`.

# Solution
```
class Solution {
    public ListNode oddEvenList(ListNode head) {
        if(head == null || head.next == null) return head;
        
        ListNode odd = head;
        ListNode even = head.next;
        
//      split into two linked lists   
        ListNode evenHead = even;
        while(odd != null && even != null){
            odd.next = even.next;
            if(even.next != null) even.next = even.next.next;
            
            if(odd.next == null) break; // when the total number of nodes are even
            odd = odd.next;
            even = even.next;
        }
        
//         merge two linked lists
        while(evenHead != null){
            odd.next = evenHead;
            odd = odd.next;
            evenHead = evenHead.next;
        }
             
        return head;
    }
}
```