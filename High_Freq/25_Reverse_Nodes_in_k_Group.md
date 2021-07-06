# 25. Reverse Nodes in k-Group
Given a linked list, reverse the nodes of a linked list  _k_  at a time and return its modified list.

_k_  is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of  _k_  then left-out nodes, in the end, should remain as it is.

**Follow up:**

-   Could you solve the problem in  `O(1)`  extra memory space?
-   You may not alter the values in the list's nodes, only nodes itself may be changed.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

**Input:** head = [1,2,3,4,5], k = 2
**Output:** [2,1,4,3,5]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

**Input:** head = [1,2,3,4,5], k = 3
**Output:** [3,2,1,4,5]

**Example 3:**

**Input:** head = [1,2,3,4,5], k = 1
**Output:** [1,2,3,4,5]

**Example 4:**

**Input:** head = [1], k = 1
**Output:** [1]

**Constraints:**

-   The number of nodes in the list is in the range  `sz`.
-   `1 <= sz <= 5000`
-   `0 <= Node.val <= 1000`
-   `1 <= k <= sz`


# Solution:
```
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        int count = 0;
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        ListNode prev = dummyHead;
        
        while(head != null){
            count++;
            if(count%k == 0){
//                 reverse
                ListNode next = head.next;
                ListNode new_prev = prev.next;
                
                reverseList(prev.next,k);
                
                prev.next = head;
                new_prev.next = next;
                prev = new_prev;
                head = next;
            }else{
                head = head.next;
            }
        }    
        return dummyHead.next;
    }
    
    
    public void reverseList(ListNode head, int k) {
        ListNode new_head = new ListNode(0);
        
        while(head != null && k > 0){
            ListNode temp = head;
            head = head.next;
            temp.next = new_head.next;
            new_head.next = temp;
            k--;
        }
       
    }
}
```
