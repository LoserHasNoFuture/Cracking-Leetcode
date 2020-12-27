# 1328. Break a Palindrome
Given a palindromic string  `palindrome`, replace  **exactly one**  character by any lowercase English letter so that the string becomes the lexicographically smallest possible string that  **isn't**  a palindrome.

After doing so, return the final string. If there is no way to do so, return the empty string.

**Example 1:**

**Input:** palindrome = "abccba"
**Output:** "aaccba"

**Example 2:**

**Input:** palindrome = "a"
**Output:** ""

**Constraints:**

-   `1 <= palindrome.length <= 1000`
-   `palindrome` consists of only lowercase English letters.

# Solution 1: Find 'a' (Beat 100%)
```
class Solution {
    public String breakPalindrome(String s) {
        if(s.length() <= 1) return new String();
        int mid = s.length()%2 == 0? -1 : s.length()/2;
        char[] word = s.toCharArray();
        
        boolean isAlla = true;
        for(int i = 0; i < word.length; i++){
            if(i == mid) continue;
            if(word[i] != 'a'){
                word[i] = 'a';
                isAlla = false;
                break;
            }
        }
        
        if(!isAlla) return new String(word);
        
        word[s.length()-1] = 'b';
        return new String(word);
    }
}
```