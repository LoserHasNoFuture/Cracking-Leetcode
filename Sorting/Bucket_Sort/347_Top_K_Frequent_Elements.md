# 347. Top K Frequent Elements
Given a non-empty array of integers, return the  **_k_**  most frequent elements.

**Example 1:**

**Input:** nums = [1,1,1,2,2,3], k = 2
**Output:** [1,2]

**Example 2:**

**Input:** nums = [1], k = 1
**Output:** [1]

**Note:**

-   You may assume  _k_  is always valid, 1 ≤  _k_  ≤ number of unique elements.
-   Your algorithm's time complexity  **must be**  better than O(_n_  log  _n_), where  _n_  is the array's size.
-   It's guaranteed that the answer is unique, in other words the set of the top k frequent elements is unique.
-   You can return the answer in any order.

# Solution 1: Priority Queue/ Quick Sort (Beat 80%)
```
class Solution {
    class Element{
        int num; 
        int cnt;
        
        Element(int _num, int _cnt){
            this.num = _num;
            this.cnt = _cnt;
        }
    }
    
    public int[] topKFrequent(int[] nums, int k) {
        HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
        
        for(int num: nums) map.put(num,map.getOrDefault(num,0)+1);
        
        PriorityQueue<Element> pq = new PriorityQueue<Element>((a,b)->(b.cnt-a.cnt));
        
        for(int num: map.keySet()) pq.offer(new Element(num,map.get(num)));
        
        int[] res = new int[k];
        
        for(int i = 0; i < k; i++) res[i] = pq.poll().num;
        
        return res;
    }
}
```

# Solution 2: Bucket Sort (Beat 99%)
```
class Solution {
    class Element{
        int num; 
        int cnt;
        
        Element(int _num, int _cnt){
            this.num = _num;
            this.cnt = _cnt;
        }
    }
    
    public int[] topKFrequent(int[] nums, int k) {
        HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
        
        for(int num: nums) map.put(num,map.getOrDefault(num,0)+1);
        
        ArrayList<Integer>[] buckets = new ArrayList[nums.length+1];
        
        for(int i = 0; i <= nums.length; i++) buckets[i] = new ArrayList<Integer>();
        
        for(int num: map.keySet()) buckets[map.get(num)].add(num);
            
        int[] res = new int[k];
        int index = 0;
        for(int i = buckets.length - 1; i >= 0; i-- ){
            if(buckets[i].size() == 0) continue;
            for(int num: buckets[i]){
                res[index] = num;
                index++;
            }
            
            if(index == k) break;
        }
        
        return res;
    }
}
```