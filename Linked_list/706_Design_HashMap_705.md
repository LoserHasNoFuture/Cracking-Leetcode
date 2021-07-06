# 706. Design HashMap
Design a HashMap without using any built-in hash table libraries.

Implement the  `MyHashMap`  class:

-   `MyHashMap()`  initializes the object with an empty map.
-   `void put(int key, int value)`  inserts a  `(key, value)`  pair into the HashMap. If the  `key`  already exists in the map, update the corresponding  `value`.
-   `int get(int key)`  returns the  `value`  to which the specified  `key`  is mapped, or  `-1`  if this map contains no mapping for the  `key`.
-   `void remove(key)`  removes the  `key`  and its corresponding  `value`  if the map contains the mapping for the  `key`.

**Example 1:**

**Input**
["MyHashMap", "put", "put", "get", "get", "put", "get", "remove", "get"]
[[], [1, 1], [2, 2], [1], [3], [2, 1], [2], [2], [2]]
**Output**
[null, null, null, 1, -1, null, 1, null, -1]

**Explanation**
MyHashMap myHashMap = new MyHashMap();
myHashMap.put(1, 1); // The map is now [[1,1]]
myHashMap.put(2, 2); // The map is now [[1,1], [2,2]]
myHashMap.get(1);    // return 1, The map is now [[1,1], [2,2]]
myHashMap.get(3);    // return -1 (i.e., not found), The map is now [[1,1], [2,2]]
myHashMap.put(2, 1); // The map is now [[1,1], [2,1]] (i.e., update the existing value)
myHashMap.get(2);    // return 1, The map is now [[1,1], [2,1]]
myHashMap.remove(2); // remove the mapping for 2, The map is now [[1,1]]
myHashMap.get(2);    // return -1 (i.e., not found), The map is now [[1,1]]

**Constraints:**

-   `0 <= key, value <= 106`
-   At most  `104`  calls will be made to  `put`,  `get`, and  `remove`.

# Solution 1: Using Array & Linked List (Beat 99%)
```
class MyHashMap {
    
    class ListNode{
        int key;
        int val;
        ListNode next;
        
        public ListNode(int _key, int _val){
            this.key = _key;
            this.val = _val;
        }
    }
    
    /** Initialize your data structure here. */
    ListNode[] map;
    int MOD = 1000;
    public MyHashMap() {
        map = new ListNode[1000];
    }
    
    /** value will always be non-negative. */
    public void put(int key, int value) {
        if(map[key%MOD] == null) {
            ListNode head = new ListNode(key,value);
            map[key%MOD] = head;
        }else{
            ListNode head = map[key%MOD];
            while(head.key != key && head.next != null) head = head.next;
            if(head.key == key) head.val = value;
            else head.next = new ListNode(key,value);
        }
    }
    
    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    public int get(int key) {
        if(map[key%MOD] == null) return -1;
        else{
            ListNode head = map[key%MOD];
            while(head != null && head.key != key) head = head.next;
            if(head == null) return -1;
            return head.val;
        }
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    public void remove(int key) {
        if(map[key%MOD] == null) return;
        else{
            ListNode head = map[key%MOD];
            if(head.key == key) {
                map[key%MOD] = head.next;
            }else{
                ListNode prev = head;
                while(head != null && head.key != key) {
                    prev = head;
                    head = head.next;
                }
                if(head != null) prev.next = head.next;
                
            }
            
            
        }
    }
}

```