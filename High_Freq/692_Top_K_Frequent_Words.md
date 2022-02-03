# 692. Top K Frequent Words
Given an array of strings  `words`  and an integer  `k`, return  _the_ `k` _most frequent strings_.

Return the answer  **sorted**  by  **the frequency**  from highest to lowest. Sort the words with the same frequency by their  **lexicographical order**.

**Example 1:**

**Input:** words = ["i","love","leetcode","i","love","coding"], k = 2
**Output:** ["i","love"]
**Explanation:** "i" and "love" are the two most frequent words.
Note that "i" comes before "love" due to a lower alphabetical order.

**Example 2:**

**Input:** words = ["the","day","is","sunny","the","the","the","sunny","is","is"], k = 4
**Output:** ["the","is","sunny","day"]
**Explanation:** "the", "is", "sunny" and "day" are the four most frequent words, with the number of occurrence being 4, 3, 2 and 1 respectively.

**Constraints:**

-   `1 <= words.length <= 500`
-   `1 <= words[i] <= 10`
-   `words[i]`  consists of lowercase English letters.
-   `k`  is in the range  `[1, The number of  **unique**  words[i]]`

**Follow-up:**  Could you solve it in  `O(n log(k))`  time and  `O(n)`  extra space?

# Solution
```
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        HashMap<String,Integer> map = new HashMap<String,Integer>();
        
        PriorityQueue<Node> pq  = new PriorityQueue<Node>();
        
        for(String str: words){
            int cnt = map.getOrDefault(str, 0) + 1;
            map.put(str, cnt);
        }
        
        Node cur;
        for(String str: map.keySet()){
            int cnt = map.get(str);
            cur = new Node(str, cnt);
            pq.offer(cur);
            if(pq.size() > k) pq.poll();
        }
        
        List<String> res = new ArrayList<String>();
        while(!pq.isEmpty()){
            res.add(0, pq.poll().str);
        }
        
        return res;
    }
    
    
    class Node implements Comparable<Node>{
        String str;
        int cnt;
        
        public Node(String _str, int _cnt){
            this.str = _str;
            this.cnt = _cnt;
        }
        
        @Override
        public int compareTo(Node that){
            return this.cnt == that.cnt? that.str.compareTo(this.str) : this.cnt - that.cnt;
        }
        
    }
}
```