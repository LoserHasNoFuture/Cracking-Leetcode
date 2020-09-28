# 2. Add Two Numbers
You are given two  **non-empty**  linked lists representing two non-negative integers. The digits are stored in  **reverse order**  and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

**Input:** (2 -> 4 -> 3) + (5 -> 6 -> 4)
**Output:** 7 -> 0 -> 8
**Explanation:** 342 + 465 = 807.

# Solution:
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