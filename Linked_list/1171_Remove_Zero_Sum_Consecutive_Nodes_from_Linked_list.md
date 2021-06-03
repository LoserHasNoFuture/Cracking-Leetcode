# 1171. Remove Zero Sum Consecutive Nodes from Linked List
Given the  `head`  of a linked list, we repeatedly delete consecutive sequences of nodes that sum to  `0`  until there are no such sequences.

After doing so, return the head of the final linked list. You may return any such answer.

(Note that in the examples below, all sequences are serializations of  `ListNode`  objects.)

**Example 1:**

**Input:** head = [1,2,-3,3,1]
**Output:** [3,1]
**Note:** The answer [1,2,1] would also be accepted.

**Example 2:**

**Input:** head = [1,2,3,-3,4]
**Output:** [1,2,4]

**Example 3:**

**Input:** head = [1,2,3,-3,-2]
**Output:** [1]

**Constraints:**

-   The given linked list will contain between  `1`  and  `1000`  nodes.
-   Each node in the linked list has  `-1000 <= node.val <= 1000`.

# Solution 1: Classical Path Sum (Beat 77%)
```
class Solution {
    public ListNode removeZeroSumSublists(ListNode head) {
        if(head == null) return null;
        HashMap<Integer,ListNode> map = new HashMap<Integer,ListNode>();
        
        ListNode newHead = new ListNode(0);
        newHead.next = head;
        map.put(0,newHead);
        
        int sum = 0;
        while(head != null){
            sum += head.val;
            if(map.containsKey(sum)){
                ListNode father = map.get(sum);
                int tmp = sum;
                ListNode inter = father.next;
                // consider case: [1,3,2,-3,-2,5,5,-5,1]
                while(inter != head){
                    tmp += inter.val;
                    map.remove(tmp);
                    inter = inter.next;
                }
                father.next = head.next;
            }else{
                map.put(sum,head);
            }
            head = head.next;
        }
        
        return newHead.next;
    }
}
```

# Solution 2: Two-Pass Solution (Beat 100%)
Refer from: [https://leetcode.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list/discuss/366319/JavaC%2B%2BPython-Greedily-Skip-with-HashMap](https://leetcode.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list/discuss/366319/JavaC%2B%2BPython-Greedily-Skip-with-HashMap)

Mainly improve the inner-while loop!!
```
class Solution {
    public ListNode removeZeroSumSublists(ListNode head) {
        int prefix = 0;
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        Map<Integer, ListNode> seen = new HashMap<>();
        seen.put(0, dummy);
        //firt pass, find the position of a pathsum last appear
        for (ListNode i = dummy; i != null; i = i.next) {
            prefix += i.val;
            seen.put(prefix, i);
        }
        // connect the first appear to last appear
        prefix = 0;
        for (ListNode i = dummy; i != null; i = i.next) {
            prefix += i.val;
            i.next = seen.get(prefix).next;
        }
        return dummy.next;
    }
}
```