# 792. Number of Matching Subsequences  392
Given a string  `s`  and an array of strings  `words`, return  _the number of_  `words[i]`  _that is a subsequence of_  `s`.

A  **subsequence**  of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

-   For example,  `"ace"`  is a subsequence of  `"abcde"`.

**Example 1:**

**Input:** s = "abcde", words = ["a","bb","acd","ace"]
**Output:** 3
**Explanation:** There are three strings in words that are a subsequence of s: "a", "acd", "ace".

**Example 2:**

**Input:** s = "dsahjpjauf", words = ["ahjpjau","ja","ahbwzgqnuk","tnmlanowax"]
**Output:** 2

**Constraints:**

-   `1 <= s.length <= 5 * 104`
-   `1 <= words.length <= 5000`
-   `1 <= words[i].length <= 50`
-   `s`  and  `words[i]`  consist of only lowercase English letters.

# Solution 1: Tricky Way (Beat 80%)
Running Time: O(m + n)
Where n is the length of String s, and m is the accumulated length of all words.
**Compared with 392, now we have three ways to solve this matching problem:**
**1) using one point, O(n), and space is O(1);**
**2) Using binary search, O(n) for the initialization and O(mlogn) for each word with length m;**
**3) this tricky solution, O(n + m), using HashMap<Character, List<String>>**

**They are optimized for three different situations:**
**1) we only have one string to match**
**2) we have a lot of string to match**
**3) the ratio between n and m are like this problem**



```
class Solution {
    public int numMatchingSubseq(String S, String[] words) {
        ArrayList[] map = new ArrayList[26];
        int res = 0;
        
        for(int i = 0; i < 26; i++) map[i] = new ArrayList<String>();
        for(String word: words){
            if(word.length() == 0) res++;
            else map[word.charAt(0)-'a'].add(word);
        }
        
        for(char c: S.toCharArray()){
            ArrayList<String> arr = map[c-'a'];
            map[c-'a'] = new ArrayList<String>();
            
            for(String word: arr){
                if(word.length() == 1) res++;
                else{
                    word = word.substring(1);
                    map[word.charAt(0)-'a'].add(word);
                }
            }
        }
        
        return res;
    }
}
```