# 716. Max Stack
Design a max stack data structure that supports the stack operations and supports finding the stack's maximum element.

Implement the  `MaxStack`  class:

-   `MaxStack()`  Initializes the stack object.
-   `void push(int x)`  Pushes element  `x`  onto the stack.
-   `int pop()`  Removes the element on top of the stack and returns it.
-   `int top()`  Gets the element on the top of the stack without removing it.
-   `int peekMax()`  Retrieves the maximum element in the stack without removing it.
-   `int popMax()`  Retrieves the maximum element in the stack and removes it. If there is more than one maximum element, only remove the  **top-most**  one.

**Example 1:**

**Input**
["MaxStack", "push", "push", "push", "top", "popMax", "top", "peekMax", "pop", "top"]
[[], [5], [1], [5], [], [], [], [], [], []]
**Output**
[null, null, null, null, 5, 5, 1, 5, 1, 5]

**Explanation**
MaxStack stk = new MaxStack();
stk.push(5);   // [**5**] the top of the stack and the maximum number is 5.
stk.push(1);   // [5, **1**] the top of the stack is 1, but the maximum is 5.
stk.push(5);   // [5, 1, **5**] the top of the stack is 5, which is also the maximum, because it is the top most one.
stk.top();     // return 5, [5, 1, **5**] the stack did not change.
stk.popMax();  // return 5, [5, **1**] the stack is changed now, and the top is different from the max.
stk.top();     // return 1, [5, **1**] the stack did not change.
stk.peekMax(); // return 5, [5, **1**] the stack did not change.
stk.pop();     // return 1, [**5**] the top of the stack and the max element is now 5.
stk.top();     // return 5, [**5**] the stack did not change.

**Constraints:**

-   `-107  <= x <= 107`
-   At most  `104`  calls will be made to  `push`,  `pop`,  `top`,  `peekMax`, and  `popMax`.
-   There will be  **at least one element**  in the stack when  `pop`,  `top`,  `peekMax`, or  `popMax`  is called.

**Follow up:** Could you come up with a solution that supports `O(1)` for each `top` call and `O(logn)` for each other call?

# Solution 1: LinkedList + MaxHeap (Beat 97%)
Can also use priority queue to do this.

```
class MaxStack {

    class ListNode{
        int val;
        int pos;
        ListNode next;
        ListNode parent;
        
        public ListNode(int _val, ListNode _next){
            this.val = _val;
            this.next = _next;
        }
    }
    
    class Node{
        int val;
        int index;
        ListNode pos;
        int time;
        
        public Node(int _val, int _index, int _time){
            this.val = _val;
            this.index = _index;
            this.time = _time;
        }
        
        public int getParent(){
            if(index == 0) return -1;
            return (this.index - 1)/2;
        }
        
        public int getLeftChild(){
            return (this.index + 1)*2 - 1;
        }
        
        public int getRightChild(){
            return (this.index + 1)*2;
        }
    }
    
    /** initialize your data structure here. */
    ListNode head;
    List<Node> maxHeap;
    int timestamp;
    public MaxStack() {
        maxHeap = new ArrayList<Node>();
        head = null;
        timestamp = 0;
    }
    
    public void add_to_heap(ListNode cur){
        Node node = new Node(cur.val,maxHeap.size(),++timestamp);
        maxHeap.add(node);
        node.pos = cur;
        cur.pos  = node.index;

        while(node.getParent() != -1){
            Node parent = maxHeap.get(node.getParent());
            if(node.val < parent.val) break;
            if(node.val == parent.val && node.time < parent.time) break;
            swap_two_nodes(node,parent);
        }
        
    }
    
    public void swap_two_nodes(Node node1, Node node2){
        maxHeap.set(node2.index,node1);
        maxHeap.set(node1.index,node2);
        int tmp = node1.index;
        node1.index = node2.index;
        node2.index = tmp;
        
        node1.pos.pos = node1.index;
        node2.pos.pos = node2.index;
    }
    
    public void remove_from_heap(int pos){
        if(pos == maxHeap.size() - 1){
            maxHeap.remove(maxHeap.size()-1);
            return;
        }
        
        Node cur = maxHeap.get(pos);
        Node last = maxHeap.get(maxHeap.size()-1);
        swap_two_nodes(last,cur);        
        maxHeap.remove(maxHeap.size()-1);

        while(last.getLeftChild() < maxHeap.size()){
            Node left = maxHeap.get(last.getLeftChild());
            int rightChild = last.getRightChild();
            Node right = rightChild < maxHeap.size() ? maxHeap.get(rightChild): null;
            
            Node to_swap = null;
            if(right == null || left.val > right.val || (left.val == right.val && left.time > right.time)){
                to_swap = left;
            }else to_swap = right;
            
            if(to_swap.val < last.val) break;
            if(to_swap.val == last.val && to_swap.time < last.time) break;
            swap_two_nodes(to_swap,last);
        }
    }
    
    public int pop() {
        if(head == null) return -1;
        int pos = head.pos;
        int val = head.val;
        delete_node(head);
        
        // System.out.println("pop");
        remove_from_heap(pos);
        return val;
    }
    
    public int popMax() {
        if(head == null) return -1;
        ListNode cur = maxHeap.get(0).pos;
        delete_node(cur);
        
        // System.out.println("popMax");
        remove_from_heap(0);
        
        return cur.val;
    }
    
    public int top() {
        if(head == null) return -1;
        return head.val;
    }
    
    public int peekMax() {
        if(head == null) return -1;
        return maxHeap.get(0).val;
    }
    
    public void push(int x) {
        ListNode cur = new ListNode(x,head);
        if(head != null) head.parent= cur;
        head = cur;
        
        add_to_heap(cur);
    }
    
    public void delete_node(ListNode cur){
        if(head == cur) {
            head = head.next;
            if(head != null) head.parent = null;
        }else {
            if(cur.next != null) cur.next.parent = cur.parent;
            cur.parent.next = cur.next;
        }
    }
}

```