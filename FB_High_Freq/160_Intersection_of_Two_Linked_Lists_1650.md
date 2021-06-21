# 160. Intersection of Two Linked Lists
Given the heads of two singly linked-lists  `headA`  and  `headB`, return  _the node at which the two lists intersect_. If the two linked lists have no intersection at all, return  `null`.

For example, the following two linked lists begin to intersect at node  `c1`:

![](https://assets.leetcode.com/uploads/2021/03/05/160_statement.png)

It is  **guaranteed**  that there are no cycles anywhere in the entire linked structure.

**Note**  that the linked lists must  **retain their original structure**  after the function returns.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/05/160_example_1_1.png)

**Input:** intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
**Output:** Intersected at '8'
**Explanation:** The intersected node's value is 8 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/05/160_example_2.png)

**Input:** intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
**Output:** Intersected at '2'
**Explanation:** The intersected node's value is 2 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [1,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/03/05/160_example_3.png)

**Input:** intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
**Output:** No intersection
**Explanation:** From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values.
Explanation: The two lists do not intersect, so return null.

**Constraints:**

-   The number of nodes of  `listA`  is in the  `m`.
-   The number of nodes of  `listB`  is in the  `n`.
-   `0 <= m, n <= 3 * 104`
-   `1 <= Node.val <= 105`
-   `0 <= skipA <= m`
-   `0 <= skipB <= n`
-   `intersectVal`  is  `0`  if  `listA`  and  `listB`  do not intersect.
-   `intersectVal == listA[skipA + 1] == listB[skipB + 1]`  if  `listA`  and  `listB`  intersect.

**Follow up:** Could you write a solution that runs in `O(n)` time and use only `O(1)` memory?

# Solution 1: HashSet, O(n) Time + O(n) Space

# Solution 2: Cut the longer list, O(n) Time + O(1) Space
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        //         Method 1: Hash Set O(n) + O(n)
        
//         Method 2: 
//          1.Notice that if they intersect, the rest of them would be the same
//          2.So find the length of each list O(n), and cut the long list to make it as long as the short list
//          3.Now, two list move together.  If they intersect, they will reach the same node at the same time!
        
        // Step 1: find length
        int len1 = find_length(headA);
        int len2 = find_length(headB);
        
//         Step 2: cut list
        if(len1>len2){
            headA = cut_list(headA,len1-len2);
        }else{
            headB = cut_list(headB,len2-len1);
        }
        
//         Step 3: start moving together
        while(headA!=null && headB != null){
            if(headA == headB){
                return headA;
            }
            headA = headA.next;
            headB = headB.next;
        }
        
        return null;
        
        
    }
    
    ListNode cut_list(ListNode head, int len){
        while(len-- > 0){
            head = head.next;
        }
        return head;
    }
    
    int find_length(ListNode head){
        int len = 0;
        while(head!=null){
            len++;
            head = head.next;
        }
        return len;
    }
}
```

# Solution 3: Connect two linkedlist together, O(1) Space + O(n) Time
If we connect to linkedlist together:
A + B
B + A
Then, the above new linked lists have the same length, and the last part of them will be the same.

```
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null) return null;

        ListNode a = headA;
        ListNode b = headB;

        while( a != b){
            a = a == null? headB : a.next;
            b = b == null? headA : b.next;    
        }

        return a;
    }
}
```