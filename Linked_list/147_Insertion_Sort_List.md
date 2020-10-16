# 147. Insertion Sort List
Sort a linked list using insertion sort.

![](https://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif)  
A graphical example of insertion sort. The partial sorted list (black) initially contains only the first element in the list.  
With each iteration one element (red) is removed from the input data and inserted in-place into the sorted list  

**Algorithm of Insertion Sort:**

1.  Insertion sort iterates, consuming one input element each repetition, and growing a sorted output list.
2.  At each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there.
3.  It repeats until no input elements remain.

  
**Example 1:**

**Input:** 4->2->1->3
**Output:** 1->2->3->4

**Example 2:**

**Input:** -1->5->3->4->0
**Output:** -1->0->3->4->5

# Solution:
```
class Solution {
    public ListNode insertionSortList(ListNode head) {
        if(head == null || head.next == null) return head;
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
     
        ListNode ptr = head;
        ListNode cur = head.next;
        ptr.next = null;
        
        while(cur != null){
            ListNode next = cur.next;
            cur.next = null;
            
            ListNode prev = dummyHead;
            if(cur.val > ptr.val) prev = ptr;
            while(prev.next != null && prev.next.val < cur.val) prev = prev.next;
            
            cur.next = prev.next;
            prev.next = cur;
            
            ptr = cur;
            cur = next;
        }
        
        return dummyHead.next;
    }
}
```