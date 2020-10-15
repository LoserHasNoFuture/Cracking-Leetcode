# 369. Plus One Linked List
Given a non-negative integer represented as  **non-empty**  a singly linked list of digits, plus one to the integer.

You may assume the integer do not contain any leading zero, except the number 0 itself.

The digits are stored such that the most significant digit is at the head of the list.

**Example :**

**Input:** [1,2,3]
**Output:** [1,2,4]

# Solution 1: Using Stack
```
class Solution {
    public ListNode plusOne(ListNode head) {
        Deque<ListNode> stack = new ArrayDeque<>();
        ListNode ptr = head;
        while(ptr != null){
            stack.push(ptr);
            ptr = ptr.next;
        }
        
        int cin = 1;
        while(!stack.isEmpty() && cin == 1){
            ListNode cur = stack.pop();
            int val = cur.val;
            cur.val = (val + cin) %10;
            cin = (val + cin) / 10;
        }

        if(cin == 1){
            ListNode newHead = new ListNode(cin);
            newHead.next = head;
            return newHead;
        }
        
        return head;
    }
}
```

# Solution 2: Reverse Linked List First
```
class Solution {
    public ListNode plusOne(ListNode head) {
        if(head == null) return head;
        
        ListNode tail = reverseList(head);
        ListNode ptr = tail;
        
        int cin = 1;
        while(cin != 0 && ptr != null){
            int val = ptr.val;
            ptr.val = (val + cin)%10;
            cin = (val + cin)/10;
            ptr = ptr.next;
        }
        
        head = reverseList(tail);
        
        if(cin != 0){
            ListNode new_head = new ListNode(cin);
            new_head.next = head;
            return new_head;
        }
        
        return head;
    }
    
    public ListNode reverseList(ListNode head){
        ListNode dummyHead = new ListNode(0);
        
        while(head != null){
            ListNode temp = head.next;
            head.next = dummyHead.next;
            dummyHead.next = head;
            head = temp;
        }
        
        return dummyHead.next;
    }
}
```

# Solution 3: Tricky Way
Find the last node who does not equals to 9.
```
class Solution {
    public ListNode plusOne(ListNode head) {
        if(head == null) return head;
        ListNode new_head = head;
        
        
        ListNode not_nine = null;
        while(head != null){
            if(head.val != 9) not_nine = head;
            head = head.next;
        }
        
        if(not_nine == null){
            not_nine = new ListNode(1);
            not_nine.next = new_head;
            new_head = not_nine;
        }else not_nine.val++;
        
        not_nine = not_nine.next;
        while(not_nine != null){
            not_nine.val = 0;
            not_nine = not_nine.next;
        }
        
        return new_head;
    }
}
```