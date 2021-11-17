# 30. Substring with Concatenation of All Words 438 567 242
You are given a string  `s`  and an array of strings  `words`  of  **the same length**. Return all starting indices of substring(s) in  `s` that is a concatenation of each word in  `words`  **exactly once**,  **in any order**, and  **without any intervening characters**.

You can return the answer in  **any order**.

**Example 1:**

**Input:** s = "barfoothefoobarman", words = ["foo","bar"]
**Output:** [0,9]
**Explanation:** Substrings starting at index 0 and 9 are "barfoo" and "foobar" respectively.
The output order does not matter, returning [9,0] is fine too.

**Example 2:**

**Input:** s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]
**Output:** []

**Example 3:**

**Input:** s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]
**Output:** [6,9,12]

**Constraints:**

-   `1 <= s.length <= 104`
-   `s`  consists of lower-case English letters.
-   `1 <= words.length <= 5000`
-   `1 <= words[i].length <= 30`
-   `words[i]` consists of lower-case English letters.

# Solution: Sliding Window
```
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        LinkedList<Integer> res = new LinkedList<Integer>();
        
        for(int i = 0; i < words[0].length(); i++){
            int start = i;
            add_to_res(start,words,res,s);
        }
        
        return res;
    }
    
    
    public void add_to_res(int start, String[] words, 
                            LinkedList<Integer> res,  String s){
        
        int wordLen = words[0].length();
        int totalLen = wordLen*words.length;
        if(s.length() < totalLen+start) return;
        
        HashMap<String,Integer> map = new HashMap<String,Integer>();
    
        
        for(int i = 0; i < words.length; i++) {
            map.put(words[i],map.getOrDefault(words[i],0)+1);
            String cur = s.substring(start+ i*wordLen, start + (i+1)*wordLen);
            int count = map.getOrDefault(cur,0) - 1;
            map.put(cur,count);
        }
            
        
        if(isAllZeros(map)) res.add(start);
        start = start+wordLen;
        int end = start + totalLen;
        
        
        while(end <= s.length()){
            String cur = s.substring(end-wordLen, end);
            map.put(cur,map.getOrDefault(cur,0) - 1);
            
            cur = s.substring(start-wordLen, start);
            map.put(cur,map.getOrDefault(cur,0) + 1);
            
            if(isAllZeros(map)) res.add(start);
            
            start = start+wordLen;
            end = end + wordLen;
        }
        
    }
    
    public boolean isAllZeros(HashMap<String,Integer> map){
        for(String key:map.keySet()){
            if(map.get(key) != 0) return false;
        }
        return true;
    }
}
```

More complicated way, but similar time complexity
```
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        LinkedList<Integer> res = new LinkedList<Integer>();
        
        for(int i = 0; i < words[0].length(); i++){
            int start = i;
            int end = i;
            add_to_res(start,end,words,res,s);
        }
        
        
        return res;
    }
    
    
    public void add_to_res(int start, int end, String[] words, 
                            LinkedList<Integer> res,  String s){
        int wordLen = words[0].length();
        int unmatched = words.length;
        
        HashMap<String,Integer> map = new HashMap<String,Integer>();
        for(int i = 0; i < words.length; i++) map.put(words[i],map.getOrDefault(words[i],0)+1);
        
        int total_length = wordLen * unmatched;
        while(end+wordLen <= s.length()){
            String cur = s.substring(end,end+wordLen);
            if(map.getOrDefault(cur,0) > 0) unmatched--;

            int count = map.getOrDefault(cur,0) - 1;
            map.put(cur, count);
            if(unmatched == 0) res.add(start);
         
            end = end+wordLen;
            if(end - start == total_length){
                cur = s.substring(start,start+wordLen);
                count = map.get(cur);
                if(count >= 0) unmatched++;
                map.put(cur, count+1);
                start = start+wordLen;
            }
            
        }
    }
}
```