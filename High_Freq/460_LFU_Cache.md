# 460. LFU Cache
Design and implement a data structure for a  [Least Frequently Used (LFU)](https://en.wikipedia.org/wiki/Least_frequently_used)  cache.

Implement the  `LFUCache`  class:

-   `LFUCache(int capacity)`  Initializes the object with the  `capacity`  of the data structure.
-   `int get(int key)`  Gets the value of the  `key`  if the  `key`  exists in the cache. Otherwise, returns  `-1`.
-   `void put(int key, int value)`  Update the value of the  `key`  if present, or inserts the  `key`  if not already present. When the cache reaches its  `capacity`, it should invalidate and remove the  **least frequently used**  key before inserting a new item. For this problem, when there is a  **tie**  (i.e., two or more keys with the same frequency), the  **least recently used**  `key`  would be invalidated.

To determine the least frequently used key, a  **use counter**  is maintained for each key in the cache. The key with the smallest  **use counter**  is the least frequently used key.

When a key is first inserted into the cache, its  **use counter**  is set to  `1`  (due to the  `put`  operation). The  **use counter**  for a key in the cache is incremented either a  `get`  or  `put`  operation is called on it.

The functions `get` and `put` must each run in  `O(1)`  average time complexity.

**Example 1:**

**Input**
["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
**Output**
[null, null, null, 1, null, -1, 3, null, -1, 3, 4]

**Explanation**
// cnt(x) = the use counter for key x
// cache=[] will show the last used order for tiebreakers (leftmost element is  most recent)
LFUCache lfu = new LFUCache(2);
lfu.put(1, 1);   // cache=[1,_], cnt(1)=1
lfu.put(2, 2);   // cache=[2,1], cnt(2)=1, cnt(1)=1
lfu.get(1);      // return 1
                 // cache=[1,2], cnt(2)=1, cnt(1)=2
lfu.put(3, 3);   // 2 is the LFU key because cnt(2)=1 is the smallest, invalidate 2.
                 // cache=[3,1], cnt(3)=1, cnt(1)=2
lfu.get(2);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,1], cnt(3)=2, cnt(1)=2
lfu.put(4, 4);   // Both 1 and 3 have the same cnt, but 1 is LRU, invalidate 1.
                 // cache=[4,3], cnt(4)=1, cnt(3)=2
lfu.get(1);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,4], cnt(4)=1, cnt(3)=3
lfu.get(4);      // return 4
                 // cache=[4,3], cnt(4)=2, cnt(3)=3

**Constraints:**

-   `0 <= capacity <= 104`
-   `0 <= key <= 105`
-   `0 <= value <= 109`
-   At most  `2 * 105` calls will be made to  `get`  and  `put`.


# Solution: 
```
int capacity, min_freq; 
HashMap<Integer,Integer> keyToVal, keyToFreq; HashMap<Integer,LinkedHashSet<Integer>> freqToKey; 
```
LinkedHashSet可以兼顾查找/删除/插入时间为O(1), 且能按照时间顺序插入/删除
注意, 在超过capacity之后, min_freq如果删没了, 不用更新 因为新插入的那个, freq = 1, 一定是min_freq 
corner case: capacity = 0;
```
class LFUCache {

    HashMap<Integer,Integer> keyToVal, keyToFreq;
    HashMap<Integer,LinkedHashSet<Integer>> freqToKey;
    int capacity, min_freq;
    public LFUCache(int _capacity) {
        capacity = _capacity;
        keyToVal = new HashMap<Integer,Integer>();
        keyToFreq = new HashMap<Integer,Integer>();
        freqToKey = new HashMap<Integer,LinkedHashSet<Integer>>();
        min_freq = 0;
    }
    
    public int get(int key) {
        if(capacity == 0) return -1;
        if(!keyToVal.containsKey(key)) return -1;
        
        increase_freq(key);
        return keyToVal.get(key);
    }
    
    public void increase_freq(int key){
        int freq = keyToFreq.get(key);
        keyToFreq.put(key,freq+1);
        
        LinkedHashSet<Integer> freqList = freqToKey.get(freq);
        freqList.remove(key);
        if(freqList.size() == 0) {
            freqToKey.remove(freq);
            if(freq == min_freq) min_freq++;
        }
        
        freqToKey.putIfAbsent(freq+1, new LinkedHashSet<Integer>());
        freqToKey.get(freq+1).add(key);
    }
    
    public void remove_min_freq(){
        LinkedHashSet<Integer> freqList = freqToKey.get(min_freq);
        int deletedKey = freqList.iterator().next();
        keyToVal.remove(deletedKey);
        keyToFreq.remove(deletedKey);
        
        freqList.remove(deletedKey);
        if(freqList.size() == 0) freqToKey.remove(min_freq);
        // we don't have to update min_freq
    }
    
    public void put(int key, int value) {
        if(capacity == 0) return;
        if(keyToVal.containsKey(key)){
            keyToVal.put(key,value);
            increase_freq(key);
        }else{
            keyToVal.put(key,value);
            keyToFreq.put(key,1);
            
            if(keyToVal.size() > capacity){
                remove_min_freq();
            }
            
            freqToKey.putIfAbsent(1, new LinkedHashSet<Integer>());
            freqToKey.get(1).add(key);
            min_freq = 1;
        }
    }
}

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache obj = new LFUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```