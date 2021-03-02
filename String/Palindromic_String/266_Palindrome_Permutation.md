# 266. Palindrome Permutation
Given a string, determine if a permutation of the string could form a palindrome.

**Example 1:**

**Input:** `"code"`
**Output:** false

**Example 2:**

**Input:** `"aab"`
**Output:** true

**Example 3:**

**Input:** `"carerac"`
**Output:** true

# Solution 1: Using HashSet (Beat 100%)
```
class Solution {
    public boolean canPermutePalindrome(String s) {
        char[] word = s.toCharArray();
        HashSet<Character> set = new HashSet<Character>();
        
        for(char c: word){
            if(!set.contains(c)) set.add(c);
            else set.remove(c);
        }
        
        return set.size() <= 1;
    }
}
```

