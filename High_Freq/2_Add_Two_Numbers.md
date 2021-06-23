# 2. Add Two Numbers
You are given two  **non-empty**  linked lists representing two non-negative integers. The digits are stored in  **reverse order**  and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

**Input:** (2 -> 4 -> 3) + (5 -> 6 -> 4)
**Output:** 7 -> 0 -> 8
**Explanation:** 342 + 465 = 807.

# Solution 1: Creating a New Linked List (Beat 100%)
```
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode newHead = new ListNode(0);
        ListNode ptr = newHead;
        int cin = 0;
        int sum = 0;
        
        while(l1 != null || l2 != null){
            int val1 = l1 == null? 0: l1.val;
            int val2 = l2 == null? 0: l2.val;
            
            sum = (val1 + val2 + cin)% 10;
            cin = (val1 + val2 + cin)/ 10;
            
            ListNode n = new ListNode(sum);
            ptr.next = n;
            ptr = ptr.next;
            l1 = l1 == null? null: l1.next;
            l2 = l2 == null? null: l2.next;
        }
        
        if(cin != 0) ptr.next = new ListNode(1);
        return newHead.next;
    }
}
```

# Solution 2: Modifying Original Linked List (Beat 100%)
```
class Solution {
//     changing one list
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        if(l1 == null) return l2;
        if(l2 == null) return l1;
        
        int carry = 0, sum = 0;
        ListNode head = l1, prev = null, cur = l1;
        
        while(l1 != null || l2 != null){
            int val1 = l1 == null? 0:l1.val;
            int val2 = l2 == null? 0:l2.val;
            
            sum = val1 + val2 + carry;
            carry = sum/10;
            
            if(l1 == null) {
                prev.next = new ListNode(sum%10);
                cur = prev.next;
            }
            else l1.val = sum%10;
            
            prev = cur;
            cur = cur.next;
            
            l1 = l1 == null?null:l1.next;
            l2 = l2 == null?null:l2.next;
        }
        
        if(carry > 0) prev.next = new ListNode(carry);
        
        
        return head;
    }
}
```