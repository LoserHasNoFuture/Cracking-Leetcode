# 445. Add Two Numbers II
You are given two  **non-empty**  linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Follow up:**  
What if you cannot modify the input lists? In other words, reversing the lists is not allowed.

**Example:**

**Input:** (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
**Output:** 7 -> 8 -> 0 -> 7

# Solution 1: Reverse List
```
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        l1 = reverseList(l1);
        l2 = reverseList(l2);
        
        ListNode dummyHead = new ListNode(0);
        int cin = 0;
        ListNode ptr = dummyHead;
        while(l1 != null || l2 != null){
            int val = cin;
            if(l1 != null){
                val += l1.val;
                l1 = l1.next;
            }
            if(l2 != null){
                val += l2.val;
                l2 = l2.next;
            }
            ptr.next = new ListNode(val%10);
            cin = val/10;
            ptr = ptr.next;
        }
        
        if(cin != 0) ptr.next = new ListNode(1);
        return reverseList(dummyHead.next);
    }
    
    
    public ListNode reverseList(ListNode head){
        if(head == null) return null;
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

# Solution 2: Using Stack
```
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        if(l1 == null) return l2;
        if(l2 == null) return l1;
        
        Deque<Integer> stack1 = new ArrayDeque<>();
        Deque<Integer> stack2 = new ArrayDeque<>();
        
        while(l1 != null || l2 != null){
            if(l1 != null){
                stack1.push(l1.val);
                l1 = l1.next;
            }
            if(l2 != null){
                stack2.push(l2.val);
                l2 = l2.next;
            }
        }
        
        ListNode dummyHead = new ListNode(0);
        int cin = 0;
        
        while(!stack1.isEmpty() || !stack2.isEmpty()){
            int val = cin;
            if(!stack1.isEmpty()) val += stack1.pop();
            if(!stack2.isEmpty()) val += stack2.pop();
            ListNode node = new ListNode(val%10);
            node.next = dummyHead.next;
            dummyHead.next = node;
            cin = val/10;
        }
        
        if(cin != 0){
            ListNode node = new ListNode(1);
            node.next = dummyHead.next;
            dummyHead.next = node;
        }
        
        return dummyHead.next;
    }
    
}
```