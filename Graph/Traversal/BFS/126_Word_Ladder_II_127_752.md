# 126. Word Ladder II 127 752
Given two words (_beginWord_  and  _endWord_), and a dictionary's word list, find all shortest transformation sequence(s) from  _beginWord_  to  _endWord_, such that:

1.  Only one letter can be changed at a time
2.  Each transformed word must exist in the word list. Note that  _beginWord_  is  _not_  a transformed word.

**Note:**

-   Return an empty list if there is no such transformation sequence.
-   All words have the same length.
-   All words contain only lowercase alphabetic characters.
-   You may assume no duplicates in the word list.
-   You may assume  _beginWord_  and  _endWord_  are non-empty and are not the same.

**Example 1:**

**Input:**
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

**Output:**
[
  ["hit","hot","dot","dog","cog"],
  ["hit","hot","lot","log","cog"]
]

**Example 2:**

**Input:**
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

**Output:** []

**Explanation:** The endWord "cog" is not in wordList, therefore no possible  transformation.

# Solution: One-End BFS Using Tree to Save Path 
```
class Solution {
    public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {        
        LinkedList<List<String>> res = new LinkedList<List<String>>();
        HashSet<String> set = new HashSet<String>();
        for(String word: wordList) set.add(word);
        if(!set.contains(endWord)) return res;
        
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        HashSet<String> visited = new HashSet<String>();
        TreeNode begin = new TreeNode(beginWord);
        queue.offer(begin);
        visited.add(beginWord);
        
        while(!queue.isEmpty() && res.isEmpty()){
            int sz = queue.size();
            HashMap<String,TreeNode> local_visited = new HashMap<String,TreeNode>();
            
            for(int i = 0; i < sz; i++){
                TreeNode cur = queue.poll();
                if(cur.val.equals(endWord)) {
                    res = Nary_tree_path(cur);
                    return res;
                } 
                
                for(int j = 0; j < cur.val.length(); j++){
                    char[] word = cur.val.toCharArray();
                    for(char c = 'a'; c<='z';c++){
                        word[j] = c;
                        String temp = new String(word);
                        if(set.contains(temp) && !visited.contains(temp)){
                            TreeNode neighbor = local_visited.get(temp);
                            if(neighbor == null) {
                                neighbor = new TreeNode(temp);
                                queue.offer(neighbor);
                            }
                            neighbor.children.add(cur);
                            local_visited.put(temp,neighbor);
                        }
                    }    
                }
            }
            for(String key : local_visited.keySet()) visited.add(key);
        }
        return new LinkedList<List<String>>();
    }
    
    
    public LinkedList<List<String>> Nary_tree_path(TreeNode cur){
        if(cur == null) return new LinkedList<List<String>>();
        LinkedList<List<String>> res = new LinkedList<List<String>>();
        if(cur.children.size() == 0) {
            LinkedList<String> li = new LinkedList<String>();
            li.add(cur.val);
            res.add(li);
            return res;
        } 
        for(TreeNode child: cur.children){
            LinkedList<List<String>> paths = Nary_tree_path(child);
            for(List<String> path: paths){
                path.add(cur.val);
                res.add(path);
            }
        }
        return res;
    }

    class TreeNode{
        public String val;
        public List<TreeNode> children;
        
        public TreeNode(String word){
            val = word;
            children = new LinkedList<TreeNode>();
        }
    }
}
```