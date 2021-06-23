# 23. Merge k Sorted Lists
You are given an array of  `k`  linked-lists  `lists`, each linked-list is sorted in ascending order.

_Merge all the linked-lists into one sorted linked-list and return it._

**Example 1:**

**Input:** lists = [[1,4,5],[1,3,4],[2,6]]
**Output:** [1,1,2,3,4,4,5,6]
**Explanation:** The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6

**Example 2:**

**Input:** lists = []
**Output:** []

**Example 3:**

**Input:** lists = [[]]
**Output:** []

**Constraints:**

-   `k == lists.length`
-   `0 <= k <= 10^4`
-   `0 <= lists[i].length <= 500`
-   `-10^4 <= lists[i][j] <= 10^4`
-   `lists[i]`  is sorted in  **ascending order**.
-   The sum of  `lists[i].length`  won't exceed  `10^4`.

# Solution 1: Recursive Merge Sort (Beat 100%)
```
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        return recursiveMerge(lists, 0, lists.length - 1);
    }
    
    public ListNode recursiveMerge(ListNode[] lists, int start, int end){
        if(start > end) return null;
        if(start == end) return lists[start];
        
        int mid = (start + end) >>> 1;
        ListNode left = recursiveMerge(lists, start, mid);
        ListNode right = recursiveMerge(lists, mid+1, end);
        
        return mergeTwoLists(left,right);
    }
    
    public ListNode mergeTwoLists(ListNode left, ListNode right){
        ListNode newHead = new ListNode(0);
        ListNode cur = newHead;
        
        while(left != null && right != null){
            if(left.val < right.val){
                cur.next = left;
                left = left.next;
            }else{
                cur.next = right;
                right = right.next;
            }
            cur = cur.next;
        }
        
        while(left != null){
            cur.next = left;
            left = left.next;
            cur = cur.next;
        }
        
        while(right != null){
            cur.next = right;
            right = right.next;
            cur = cur.next;
        }
        
        return newHead.next;
    }
}
```