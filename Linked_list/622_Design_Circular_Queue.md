# 622. Design Circular Queue
Design your implementation of the circular queue. The circular queue is a linear data structure in which the operations are performed based on FIFO (First In First Out) principle and the last position is connected back to the first position to make a circle. It is also called "Ring Buffer".

One of the benefits of the circular queue is that we can make use of the spaces in front of the queue. In a normal queue, once the queue becomes full, we cannot insert the next element even if there is a space in front of the queue. But using the circular queue, we can use the space to store new values.

Implementation the  `MyCircularQueue`  class:

-   `MyCircularQueue(k)`  Initializes the object with the size of the queue to be  `k`.
-   `int Front()`  Gets the front item from the queue. If the queue is empty, return  `-1`.
-   `int Rear()`  Gets the last item from the queue. If the queue is empty, return  `-1`.
-   `boolean enQueue(int value)`  Inserts an element into the circular queue. Return  `true`  if the operation is successful.
-   `boolean deQueue()`  Deletes an element from the circular queue. Return  `true`  if the operation is successful.
-   `boolean isEmpty()`  Checks whether the circular queue is empty or not.
-   `boolean isFull()`  Checks whether the circular queue is full or not.

You must solve the problem without using the built-in queue data structure in your programming language.

**Example 1:**

**Input**
["MyCircularQueue", "enQueue", "enQueue", "enQueue", "enQueue", "Rear", "isFull", "deQueue", "enQueue", "Rear"]
[[3], [1], [2], [3], [4], [], [], [], [4], []]
**Output**
[null, true, true, true, false, 3, true, true, true, 4]

**Explanation**
```
MyCircularQueue myCircularQueue = new MyCircularQueue(3);
myCircularQueue.enQueue(1); // return True
myCircularQueue.enQueue(2); // return True
myCircularQueue.enQueue(3); // return True
myCircularQueue.enQueue(4); // return False
myCircularQueue.Rear();     // return 3
myCircularQueue.isFull();   // return True
myCircularQueue.deQueue();  // return True
myCircularQueue.enQueue(4); // return True
myCircularQueue.Rear();     // return 4
```

**Constraints:**

-   `1 <= k <= 1000`
-   `0 <= value <= 1000`
-   At most  `3000`  calls will be made to `enQueue`,  `deQueue`, `Front`, `Rear`, `isEmpty`, and `isFull`.

# Solution 1: LinkedList (Beat 20%)
```
class MyCircularQueue {
    
    class ListNode{
        int val;
        ListNode prev;
        ListNode next;
        
        public ListNode(int _val){
            this.val = _val;
        }
    
    }
    
    ListNode head;
    int capacity, cnt;
    public MyCircularQueue(int k) {
        head = null;
        capacity = k;
        cnt = 0;
    }
    
    public boolean enQueue(int value) {
        if(isFull()) return false;
        cnt++;
        ListNode cur = new ListNode(value);
        if(head == null){
            head = cur;
            cur.next = cur;
            cur.prev = cur;
        }else{
            head.prev.next = cur;
            cur.prev = head.prev;
            
            cur.next = head;
            head.prev = cur;
            
            cur = head;
        }
        return true;
    }
    
    public boolean deQueue() {
        if(isEmpty()) return false;
        cnt--;
        if(isEmpty()) head = null;
        else{
            head.next.prev = head.prev;
            head.prev.next = head.next;
            head = head.next;
        }
        return true;
    }
    
    public int Front() {
        if(isEmpty()) return -1;
        else return head.val;
    }
    
    public int Rear() {
        if(isEmpty()) return -1;
        else return head.prev.val;
    }
    
    public boolean isEmpty() {
        return cnt == 0;
    }
    
    public boolean isFull() {
        return cnt == capacity;
    }
}
```


# Solution 2: Using Array, Because the capacity is fixed. (Beat 20%)
```
class MyCircularQueue {
    
    int[] arr;
    int capacity, cnt, head, tail;
    public MyCircularQueue(int k) {
        arr = new int[k];
        head = -1;
        tail = -1;
        capacity = k;
        cnt = 0;
    }
    
    public boolean enQueue(int value) {
        if(isFull()) return false;
        cnt++;
        tail = (tail+1)%capacity;
        arr[tail] = value;
        if(head == -1) head = tail;
        return true;
    }
    
    public boolean deQueue() {        
        if(isEmpty()) return false;
        cnt--;
        if(isEmpty()) {
            head = -1;
            tail = -1;
        }else{
            head = (head+1)%capacity;
        }
        return true;
    }
    
    public int Front() {
        if(isEmpty()) return -1;
        else return arr[head];
    }
    
    public int Rear() {
        if(isEmpty()) return -1;
        else return arr[tail];
    }
    
    public boolean isEmpty() {
        return cnt == 0;
    }
    
    public boolean isFull() {
        return cnt == capacity;
    }
}

```