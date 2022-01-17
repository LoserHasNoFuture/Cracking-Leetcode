# 708. Insert into a Sorted Circular Linked List
Given a Circular Linked List node, which is sorted in ascending order, write a function to insert a value  `insertVal`  into the list such that it remains a sorted circular list. The given node can be a reference to any single node in the list and may not necessarily be the smallest value in the circular list.

If there are multiple suitable places for insertion, you may choose any place to insert the new value. After the insertion, the circular list should remain sorted.

If the list is empty (i.e., the given node is  `null`), you should create a new single circular list and return the reference to that single node. Otherwise, you should return the originally given node.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/01/19/example_1_before_65p.jpg)  

**Input:** head = [3,4,1], insertVal = 2
**Output:** [3,4,1,2]
**Explanation:** In the figure above, there is a sorted circular list of three elements. You are given a reference to the node with value 3, and we need to insert 2 into the list. The new node should be inserted between node 1 and node 3. After the insertion, the list should look like this, and we should still return node 3.

![](https://assets.leetcode.com/uploads/2019/01/19/example_1_after_65p.jpg)

**Example 2:**

**Input:** head = [], insertVal = 1
**Output:** [1]
**Explanation:** The list is empty (given head is `null`). We create a new single circular list and return the reference to that single node.

**Example 3:**

**Input:** head = [1], insertVal = 0
**Output:** [1,0]

**Constraints:**

-   `0 <= Number of Nodes <= 5 * 104`
-   `-106  <= Node.val, insertVal <= 106`

# Solution 1: Direct Solution (Beat 100%)
```
class Solution {
    public Node insert(Node head, int insertVal) {
        Node add = new Node(insertVal);
        if(head == null){
            add.next = add;
            return add;
        }
        Node cur = head;
        while(true) {
            if(cur.val <= cur.next.val){
                if((add.val < cur.val || add.val > cur.next.val)) cur = cur.next;
                else break;
            }else{
                if(add.val <= cur.next.val || add.val >= cur.val) break;
                else cur = cur.next;
            }
            
            // 绕了一圈都没有找到插入的位置
            if(cur == head) break;
        }
        
        add.next = cur.next;
        cur.next = add;
        
        return head;
    }
}
```