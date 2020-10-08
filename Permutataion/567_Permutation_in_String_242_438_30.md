# 567. Permutation in String 242 438 
Given two strings  **s1**  and  **s2**, write a function to return true if  **s2**  contains the permutation of  **s1**. In other words, one of the first string's permutations is the  **substring**  of the second string.

**Example 1:**

**Input:** s1 = "ab" s2 = "eidbaooo"
**Output:** True
**Explanation:** s2 contains one permutation of s1 ("ba").

**Example 2:**

**Input:**s1= "ab" s2 = "eidboaoo"
**Output:** False

**Constraints:**

-   The input strings only contain lower case letters.
-   The length of both given strings is in range [1, 10,000].

# Solution 1: Sliding Window
2 Ways to code:
1. Easier to understand
```
public class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int len1 = s1.length(), len2 = s2.length();
        if (len1 > len2) return false;
        
        int[] count = new int[26];
        for (int i = 0; i < len1; i++) {
            count[s1.charAt(i) - 'a']++;
            count[s2.charAt(i) - 'a']--;
        }
        if (allZero(count)) return true;
        
        for (int i = len1; i < len2; i++) {
            count[s2.charAt(i) - 'a']--;
            count[s2.charAt(i - len1) - 'a']++;
            if (allZero(count)) return true;
        }
        
        return false;
    }
    
    private boolean allZero(int[] count) {
        for (int i = 0; i < 26; i++) {
            if (count[i] != 0) return false;
        }
        return true;
    }
}
```

2. More complicated
```
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int[] map = new int[26];
        for(int i = 0; i < s1.length(); i++) map[s1.charAt(i) - 'a']++;
        
        int start = 0;
        int end = 0;
        int unmatched = s1.length();
        
        while(end<s2.length()){
            if(map[s2.charAt(end)-'a'] > 0) unmatched--;
            map[s2.charAt(end)-'a']--;
            if(unmatched == 0) return true;
            end++;
            if(end - start == s1.length()){
                if(map[s2.charAt(start)-'a'] >= 0) unmatched++;
                map[s2.charAt(start)-'a']++;
                start++;
            }
        }
        
        return false;
    }
}
```