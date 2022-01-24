# 211. Design Add and Search Words Data Structure 208
Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the  `WordDictionary`  class:

-   `WordDictionary()` Initializes the object.
-   `void addWord(word)`  Adds  `word`  to the data structure, it can be matched later.
-   `bool search(word)` Returns  `true`  if there is any string in the data structure that matches  `word` or  `false`  otherwise.  `word`  may contain dots  `'.'`  where dots can be matched with any letter.

**Example:**

**Input**
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
**Output**
[null,null,null,null,false,true,true,true]

**Explanation**
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("bad");
wordDictionary.addWord("dad");
wordDictionary.addWord("mad");
wordDictionary.search("pad"); // return False
wordDictionary.search("bad"); // return True
wordDictionary.search(".ad"); // return True
wordDictionary.search("b.."); // return True

**Constraints:**

-   `1 <= word.length <= 500`
-   `word`  in  `addWord`  consists lower-case English letters.
-   `word`  in  `search`  consist of `'.'`  or lower-case English letters.
-   At most  `50000` calls will be made to  `addWord` and  `search`.

# Solution: Trie
```
class WordDictionary {

    class TreeNode{
        boolean isWord;
        TreeNode[] children;
        
        public TreeNode(){
            isWord = false;
            children = new TreeNode[26];
        }
    }
    
    TreeNode root;
    public WordDictionary() {
        root = new TreeNode();
    }
    
    public void addWord(String word) {
        TreeNode cur = root;
        for(char c: word.toCharArray()){
            int child = c - 'a';
            if(cur.children[child] == null) {
                cur.children[child] = new TreeNode();
            }
            cur = cur.children[child];
        }
        cur.isWord = true;
    }
    
    public boolean search(String word) {
        TreeNode cur = root;
        return dfs(cur, word, 0);
    }
    
    public boolean dfs(TreeNode cur, String word, int index){
        if(index == word.length()) return cur.isWord;
        
        
        if(word.charAt(index) == '.'){
            for(TreeNode child: cur.children){
                if(child != null && dfs(child, word, index+1)) {
                    return true;
                }
            }
            return false;
        }else{
            int child = word.charAt(index) - 'a';
            if(cur.children[child] == null) return false;
            return dfs(cur.children[child], word, index+1);
        }

        
    }
}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */
```