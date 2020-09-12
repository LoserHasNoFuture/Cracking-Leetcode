# 234. Palindrome Linked List
Given a singly linked list, determine if it is a palindrome.

**Example 1:**

**Input:** 1->2
**Output:** false

**Example 2:**

**Input:** 1->2->2->1
**Output:** true

**Follow up:**  
Could you do it in O(n) time and O(1) space?

# Solution
1. find the medium of the linked list
2. reverse the first part and compare with the last part

```
class Solution {
    public boolean isPalindrome(ListNode head) {
        if(head == null || head.next == null) return true;
        ListNode slow = head;
        ListNode fast = head.next;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        
        ListNode mid_head = slow.next;
        slow.next = null;
        
        head = reverseList(head); 
        if(fast == null) head = head.next;
        
        while(head != null && mid_head != null){
            if(head.val != mid_head.val) return false;
            head = head.next;
            mid_head = mid_head.next;
        }
        return true;
    }   
    
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