# 1047. Remove All Adjacent Duplicates In String 1209 283
You are given a string  `s`  consisting of lowercase English letters. A  **duplicate removal**  consists of choosing two  **adjacent**  and  **equal**  letters and removing them.

We repeatedly make  **duplicate removals**  on  `s`  until we no longer can.

Return  _the final string after all such duplicate removals have been made_. It can be proven that the answer is  **unique**.

**Example 1:**

**Input:** s = "abbaca"
**Output:** "ca"
**Explanation:** 
For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".

**Example 2:**

**Input:** s = "azxxzy"
**Output:** "ay"

**Constraints:**

-   `1 <= s.length <= 105`
-   `s`  consists of lowercase English letters.

# Solution 1: Direct Solution (Beat 100%)
```
class Solution {
    public String removeDuplicates(String s) {
        if(s.length() <= 1) return s;
        // s = "abbaca"
        char[] word = s.toCharArray();
        int index = 0;
        
        for(int i = 1; i < word.length; i++){
            if(index < 0) swap(word,++index, i);
            else if(word[i] != word[index]){
                swap(word,++index, i);
            }else index--;
        }
        
        return (new String(word)).substring(0,index+1);
    }
    
    public void swap(char[] word, int a, int b){
        char tmp = word[a];
        word[a] = word[b];
        word[b] = tmp;
    }
}
```

Better Coding Implementation:
```
class Solution {
    public String removeDuplicates(String s) {
        if(s.length() <= 1) return s;
        char[] word = s.toCharArray();
        int index = 0;
        
        for(int i = 1; i < word.length; i++){
            if(index < 0 || word[i] != word[index]) word[++index] = word[i];
            else index--;
        }
        
        return (new String(word)).substring(0,index+1);
    }
    
}
```