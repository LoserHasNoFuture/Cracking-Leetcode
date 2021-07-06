# 1019. Next Greater Node In Linked List
We are given a linked list with `head` as the first node. Let's number the nodes in the list:  `node_1, node_2, node_3, ...`  etc.

Each node may have a  _next larger_  **value**: for  `node_i`, `next_larger(node_i)` is the  `node_j.val`  such that  `j > i`,  `node_j.val > node_i.val`, and  `j`  is the smallest possible choice. If such a  `j` does not exist, the next larger value is  `0`.

Return an array of integers `answer`, where  `answer[i] = next_larger(node_{i+1})`.

Note that in the example  **inputs** (not outputs) below, arrays such as  `[2,1,5]` represent the serialization of a linked list with a head node value of 2, second node value of 1, and third node value of 5.

**Example 1:**

**Input:** [2,1,5]
**Output:** [5,5,0]

**Example 2:**

**Input:** [2,7,4,3,5]
**Output:** [7,0,5,5,0]

**Example 3:**

**Input:** [1,7,5,1,9,2,5,1]
**Output:** [7,9,9,9,0,5,0,0]

**Note:**

1.  `1 <= node.val <= 10^9` for each node in the linked list.
2.  The given list has length in the range  `[0, 10000]`.

# Solution 1: Reverse + Stack (Beat 100%)
Stack is implemented by Array.
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    int len = 0;
    public int[] nextLargerNodes(ListNode head) {
//         reverse Linked List
        head = reverseList(head);
        
        int[] res = new int[len];
        int[] stack = new int[len];
        int stack_index = -1, res_index = len;
        
        while(head != null){
            while(stack_index >= 0 && stack[stack_index] <= head.val) stack_index--;
            
            if(stack_index >= 0) res[--res_index] = stack[stack_index];
            else res[--res_index] = 0;
            
            stack[++stack_index] = head.val;
            head = head.next;
        }
        
        return res;
    }
    
    public ListNode reverseList(ListNode head){
        len = 0;
        ListNode newHead = new ListNode(0);
        
        while(head != null){
            ListNode tmp = head.next;
            head.next = newHead.next;
            newHead.next = head;
            head = tmp;
            len++;
        }
        
        return newHead.next;
    }
    
    
}
```

# Solution 2: Direct use stack (Beat 100%)
```
class Solution {
    
    public int[] nextLargerNodes(ListNode head) {
        List<Integer> nums = new ArrayList<Integer>();
        parseAllvalues(head,nums);
        int[] res = new int[nums.size()];
        Deque<Integer> stack = new ArrayDeque<Integer>();
        int index = 0;
        
        for(int num:nums){
            while(!stack.isEmpty() && nums.get(stack.peek()) < num){
                res[stack.pop()] = num;
            }
            
            stack.push(index++);    
        }
        
        while(!stack.isEmpty()) res[stack.pop()] = 0;
        
        return res;
    }
    
    public void parseAllvalues(ListNode head, List<Integer> arr){
        
        while(head != null) {
            arr.add(head.val);
            head = head.next;
        }
        
    }


}
```

# Solution 3: Dynamic Programming (Beat 100%)
```
class Solution {
    
    public int[] nextLargerNodes(ListNode head) {
        List<Integer> nums = new ArrayList<Integer>();
        parseAllvalues(head,nums);    
        int[] res = new int[nums.size()];
        
        for(int i = nums.size()-2; i >= 0; i--){
            for(int j = i+1; j < nums.size(); j++){
                if(nums.get(i) < nums.get(j)){
                    res[i] = nums.get(j);
                    break;
                }
                
                if(nums.get(i) < res[j] || res[j]== 0){
                    res[i] = res[j];
                    break;
                }
                
            }
        }
        
        return res;
    }
    
    public void parseAllvalues(ListNode head, List<Integer> arr){
        
        while(head != null) {
            arr.add(head.val);
            head = head.next;
        }
        
    }

    
}
```