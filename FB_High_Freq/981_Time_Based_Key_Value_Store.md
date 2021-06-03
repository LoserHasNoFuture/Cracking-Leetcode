# 981. Time Based Key-Value Store
Create a timebased key-value store class `TimeMap`, that supports two operations.

1.  `set(string key, string value, int timestamp)`

-   Stores the  `key`  and  `value`, along with the given  `timestamp`.

2.  `get(string key, int timestamp)`

-   Returns a value such that  `set(key, value, timestamp_prev)`  was called previously, with  `timestamp_prev <= timestamp`.
-   If there are multiple such values, it returns the one with the largest  `timestamp_prev`.
-   If there are no values, it returns the empty string (`""`).

**Example 1:**

**Input:** inputs = ["TimeMap","set","get","get","set","get","get"], inputs = [[],["foo","bar",1],["foo",1],["foo",3],["foo","bar2",4],["foo",4],["foo",5]]
**Output:** [null,null,"bar","bar",null,"bar2","bar2"]
**Explanation: **TimeMap kv;   
kv.set("foo", "bar", 1); // store the key "foo" and value "bar" along with timestamp = 1   
kv.get("foo", 1);  // output "bar"   
kv.get("foo", 3); // output "bar" since there is no value corresponding to foo at timestamp 3 and timestamp 2, then the only value is at timestamp 1 ie "bar"   
kv.set("foo", "bar2", 4);   
kv.get("foo", 4); // output "bar2"   
kv.get("foo", 5); //output "bar2" 

**Example 2:**

**Input:** inputs = ["TimeMap","set","set","get","get","get","get","get"], inputs = [[],["love","high",10],["love","low",20],["love",5],["love",10],["love",15],["love",20],["love",25]]
**Output:** [null,null,null,"","high","high","low","low"]

**Note:**

1.  All key/value strings are lowercase.
2.  All key/value strings have length in the range `[1, 100]`
3.  The  `timestamps`  for all  `TimeMap.set`  operations are strictly increasing.
4.  `1 <= timestamp <= 10^7`
5.  `TimeMap.set`  and  `TimeMap.get` functions will be called a total of  `120000`  times (combined) per test case.

# Solution 1: HashMap and Binary Search
```
class TimeMap {

    /** Initialize your data structure here. */
    class Value{
        String val;
        int timestamp;
        Value(String _val, int _time){
            this.val = _val;
            this.timestamp = _time;
        }
    }
    
    HashMap<String,ArrayList<Value>> map;
    public TimeMap() {
        map = new HashMap<String,ArrayList<Value>>();
    }
    
    public void set(String key, String value, int timestamp) {
        ArrayList<Value> arr;
        if(map.containsKey(key)) arr = map.get(key);            
        else{
            arr = new ArrayList<Value>();
            map.put(key,arr);
        }
        arr.add(new Value(value,timestamp));
    }
    
    public String get(String key, int timestamp) {
        ArrayList<Value> arr = map.get(key);    
        return binary_search(0, arr.size() - 1, timestamp, arr);
    }
    
    public String binary_search(int start, int end, int time, ArrayList<Value> arr){
        if(arr.get(start).timestamp > time) return "";
        if(arr.get(end).timestamp <= time) return arr.get(end).val;
        
        while(start < end){
            int mid = (start + end + 1) >>> 1;
            if(arr.get(mid).timestamp == time) return arr.get(mid).val;
            if(arr.get(mid).timestamp > time) end = mid - 1;
            else start = mid;
        }
        return arr.get(start).val;
    }
}
```
