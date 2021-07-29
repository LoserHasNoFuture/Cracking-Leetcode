# 127. Word Ladder 126 752
Given two words (_beginWord_  and  _endWord_), and a dictionary's word list, find the length of shortest transformation sequence from  _beginWord_  to  _endWord_, such that:

1.  Only one letter can be changed at a time.
2.  Each transformed word must exist in the word list.

**Note:**

-   Return 0 if there is no such transformation sequence.
-   All words have the same length.
-   All words contain only lowercase alphabetic characters.
-   You may assume no duplicates in the word list.
-   You may assume  _beginWord_  and  _endWord_  are non-empty and are not the same.

**Example 1:**

**Input:**
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

**Output:** 5

**Explanation:** As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.

**Example 2:**

**Input:**
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

**Output:** 0

**Explanation:** The endWord "cog" is not in wordList, therefore no possible  transformation.

# Solution: One-End BFS
```
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        HashSet<String> set = new HashSet<String>();
        for(String word: wordList) set.add(word);
        if(!set.contains(endWord)) return 0;
        
        int res = 1;
        Queue<String> queue = new LinkedList<String>();
        HashSet<String> visited = new HashSet<String>();
        queue.offer(beginWord);
        visited.add(beginWord);
        
        while(!queue.isEmpty()){
            int sz = queue.size();
            
            for(int i = 0; i < sz; i++){
                String cur = queue.poll();
                if(cur.equals(endWord)) return res;
                
                for(int j = 0; j < cur.length(); j++){
                    char[] word = cur.toCharArray();
                    for(char c = 'a'; c<='z';c++){
                        word[j] = c;
                        String temp = new String(word);
                        if(set.contains(temp) && !visited.contains(temp)){
                            visited.add(temp);
                            queue.add(temp);
                        }
                    }    
                }   
            }
            res++;
        }
        
        return 0;
    }
}
```
Another way (Using more Space to save Time):
```
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        if(!wordList_contains_endWord(endWord,wordList)) return 0;
        // key is: h*t,  value is a list contains: hit, hat, hot (who exist in wordList)
        HashMap<String,LinkedList<String>> dict = new HashMap<String,LinkedList<String>>();
        popular_dictionary(wordList,dict);
        
        int res = 1;
        Queue<String> queue = new LinkedList<String>();
        HashSet<String> visited = new HashSet<String>();
        queue.offer(beginWord);
        visited.add(beginWord);
        
        while(!queue.isEmpty()){
            int sz = queue.size();
            
            for(int i = 0; i < sz; i++){
                String cur = queue.poll();
                if(cur.equals(endWord)) return res;
                String temp = "*" + cur.substring(1);
                add_to_queue(queue,visited,temp,dict);
                for(int j = 1; j < cur.length(); j++){
                    temp = cur.substring(0,j) + "*" + cur.substring(j+1);
                    add_to_queue(queue,visited,temp,dict);
                }
            }
            
            res++;
        }
        
        return 0;
    }
    
    public void add_to_queue(Queue<String> queue, HashSet<String> visited, 
                               String temp, HashMap<String,LinkedList<String>> dict){
        LinkedList<String> li = dict.get(temp);
        if(li != null){
            for(String s : li){
                if(!visited.contains(s)){
                    queue.add(s);
                    visited.add(s);
                }
            }
        }
    }
    
    public boolean wordList_contains_endWord(String endWord, List<String> wordList){
        for(String word : wordList){
            if(word.equals(endWord)) return true;
        }
        return false;
    }
    
    public void popular_dictionary(List<String> wordList, HashMap<String,LinkedList<String>> dict){
        for(int i = 0; i < wordList.size(); i++) {
            String cur = wordList.get(i);
            String temp = "*" + cur.substring(1);
            LinkedList<String> li = dict.getOrDefault(temp, new LinkedList<String>());
            li.add(cur);
            dict.put(temp,li);
            for(int j = 1; j < cur.length(); j++){
                temp = cur.substring(0,j) + "*" + cur.substring(j+1);
                li = dict.getOrDefault(temp, new LinkedList<String>());
                li.add(cur);
                dict.put(temp,li);
            }
        }
    }
}
```

# Solution: Two-End BFS
```
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        HashSet<String> set = new HashSet<String>();
        for(String word: wordList) set.add(word);
        if(!set.contains(endWord)) return 0;
        
        int res = 1;
        HashSet<String> queue = new HashSet<String>();
        HashSet<String> queue2 = new HashSet<String>();
        HashSet<String> visited = new HashSet<String>();
        HashSet<String> visited2 = new HashSet<String>();
        queue.add(beginWord);
        visited.add(beginWord);
        queue2.add(endWord);
        visited2.add(endWord);
        
        while(!queue.isEmpty()){
            if(queue2.size() < queue.size()){
                HashSet<String> temp = queue2;
                queue2 = queue;
                queue = temp;
                temp = visited2;
                visited2 = visited;
                visited = temp;
            }
            
            HashSet<String> temp = new HashSet<String>();            
            for(String cur : queue){
                if(queue2.contains(cur)) return res;
                for(int j = 0; j < cur.length(); j++){
                    char[] chars = cur.toCharArray();
                    for(char c = 'a'; c<='z';c++){
                        chars[j] = c;
                        String word = new String(chars);
                        if(set.contains(word) && !visited.contains(word)){
                            visited.add(word);
                            temp.add(word);
                        }
                    }    
                }
            }
            queue = temp;
            res++;
        }
        return 0;
    }
}
```