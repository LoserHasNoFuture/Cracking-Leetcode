# 146. LRU Cache
Design a data structure that follows the constraints of a  **[Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)**.

Implement the  `LRUCache`  class:

-   `LRUCache(int capacity)`  Initialize the LRU cache with  **positive**  size  `capacity`.
-   `int get(int key)`  Return the value of the  `key`  if the key exists, otherwise return  `-1`.
-   `void put(int key, int value)` Update the value of the  `key`  if the  `key`  exists. Otherwise, add the  `key-value`  pair to the cache. If the number of keys exceeds the  `capacity`  from this operation,  **evict**  the least recently used key.

**Follow up:**  
Could you do  `get`  and  `put`  in  `O(1)`  time complexity?

**Example 1:**

**Input**
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
**Output**
[null, null, null, 1, null, -1, null, -1, 3, 4]

**Explanation**
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4

**Constraints:**

-   `1 <= capacity <= 3000`
-   `0 <= key <= 3000`
-   `0 <= value <= 104`
-   At most  `3 * 104`  calls will be made to  `get`  and  `put`.

# Solution 1: Double LinkedList + HashMap (Beat 100%)
Very classical solution, refers from: [https://leetcode.com/problems/lru-cache/discuss/45911/Java-Hashtable-%2B-Double-linked-list-(with-a-touch-of-pseudo-nodes)](https://leetcode.com/problems/lru-cache/discuss/45911/Java-Hashtable-%2B-Double-linked-list-(with-a-touch-of-pseudo-nodes))
```
import java.util.Hashtable;


public class LRUCache {

    class DLinkedNode {
      int key;
      int value;
      DLinkedNode pre;
      DLinkedNode post;
    }

    /**
     * Always add the new node right after head;
     */
    private void addNode(DLinkedNode node) {

      node.pre = head;
      node.post = head.post;

      head.post.pre = node;
      head.post = node;
    }

    /**
     * Remove an existing node from the linked list.
     */
    private void removeNode(DLinkedNode node){
      DLinkedNode pre = node.pre;
      DLinkedNode post = node.post;

      pre.post = post;
      post.pre = pre;
    }

    /**
     * Move certain node in between to the head.
     */
    private void moveToHead(DLinkedNode node){
      this.removeNode(node);
      this.addNode(node);
    }

    // pop the current tail. 
    private DLinkedNode popTail(){
      DLinkedNode res = tail.pre;
      this.removeNode(res);
      return res;
    }

    private Hashtable<Integer, DLinkedNode> 
      cache = new Hashtable<Integer, DLinkedNode>();
    private int count;
    private int capacity;
    private DLinkedNode head, tail;

    public LRUCache(int capacity) {
      this.count = 0;
      this.capacity = capacity;

      head = new DLinkedNode();
      head.pre = null;

      tail = new DLinkedNode();
      tail.post = null;

      head.post = tail;
      tail.pre = head;
    }

    public int get(int key) {

      DLinkedNode node = cache.get(key);
      if(node == null){
        return -1; // should raise exception here.
      }

      // move the accessed node to the head;
      this.moveToHead(node);

      return node.value;
    }


    public void put(int key, int value) {
      DLinkedNode node = cache.get(key);

      if(node == null){

        DLinkedNode newNode = new DLinkedNode();
        newNode.key = key;
        newNode.value = value;

        this.cache.put(key, newNode);
        this.addNode(newNode);

        ++count;

        if(count > capacity){
          // pop the tail
          DLinkedNode tail = this.popTail();
          this.cache.remove(tail.key);
          --count;
        }
      }else{
        // update the value.
        node.value = value;
        this.moveToHead(node);
      }
    }

}
```


# Solution 2: Single LinkedList + HashMap (Beat 100%)
``` 
class LRUCache {
    int capacity = 0;
    int count = 0;
    HashMap<Integer, ListNode> map = new HashMap<Integer, ListNode>();
    ListNode head = null;
    ListNode tail = null;
    
    class ListNode{
        int val, key;
        ListNode next = null;
        ListNode(int _key, int _val){
            this.key = _key;
            this.val = _val;
        }
    }
    
    public LRUCache(int _capacity) {
        this.capacity = _capacity;
    }
    
    public int get(int key) {
        if(map.containsKey(key)){
            ListNode old = map.get(key);
            int res = old.val;
            move_to_tail(old);
            
            return res;
        } 
        return -1;
    }
    
    public void move_to_tail(ListNode old){
        if(old == tail) return;
        int key = old.key;
        ListNode node = new ListNode(key, old.val);
        map.put(key,node);
        tail.next = node;
        tail = node;

        old.key = old.next.key;
        old.val = old.next.val;
        map.put(old.key,old);
        old.next = old.next.next;
    }
    
    public void put(int key, int value) {
        if(map.containsKey(key)) {
            ListNode node = map.get(key);
            node.val = value;
            move_to_tail(node);
        }else{
            ListNode node = new ListNode(key, value);
            map.put(key,node);
            count++;
            if(head == null){
                head = node; tail = node;
            }else{
                tail.next = node;
                tail = node;
            }
            
            if(count > capacity) remove_lru();
        }
    }
    
    public void remove_lru(){
        map.remove(head.key);
        head = head.next;
        count--;
    }
    
}
```