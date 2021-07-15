# 345. Reverse Vowels of a String
Given a string  `s`, reverse only all the vowels in the string and return it.

The vowels are  `'a'`,  `'e'`,  `'i'`,  `'o'`, and  `'u'`, and they can appear in both cases.

**Example 1:**

**Input:** s = "hello"
**Output:** "holle"

**Example 2:**

**Input:** s = "leetcode"
**Output:** "leotcede"

**Constraints:**

-   `1 <= s.length <= 3 * 105`
-   `s`  consist of  **printable ASCII**  characters.

# Solution 1: Two Pointers (Beat 100%)
```
class Solution {
    public String reverseVowels(String s) {
        int start = 0, end = s.length() - 1;
        boolean[] map = new  boolean[256];
        map['a'] = true; map['e'] = true; 
        map['i'] = true; map['o'] = true;
        map['u'] = true;
        map['A'] = true; map['E'] = true; 
        map['I'] = true; map['O'] = true;
        map['U'] = true;
        
        char[] word = s.toCharArray();
        
        while(start < end){
            while(start < end && !map[word[start]]) start++;
            while(start < end && !map[word[end]]) end--;
            
            swap(word,start,end);
            start++;end--;
        }
        
        return new String(word);
    }
    
    public void swap(char[] word, int a, int b){
        char tmp = word[a];
        word[a] = word[b];
        word[b] = tmp;
    }
}
```