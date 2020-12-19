# 1593. Split a String Into the Max Number of Unique Substrings
Given a string `s`, return  _the maximum number of unique substrings that the given string can be split into_.

You can split string `s`  into any list of **non-empty substrings**, where the concatenation of the substrings forms the original string. However, you must split the substrings such that all of them are  **unique**.

A  **substring**  is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "ababccc"
**Output:** 5
**Explanation**: One way to split maximally is ['a', 'b', 'ab', 'c', 'cc']. Splitting like ['a', 'b', 'a', 'b', 'c', 'cc'] is not valid as you have 'a' and 'b' multiple times.

**Example 2:**

**Input:** s = "aba"
**Output:** 2
**Explanation**: One way to split maximally is ['a', 'ba'].

**Example 3:**

**Input:** s = "aa"
**Output:** 1
**Explanation**: It is impossible to split the string any further.

**Constraints:**

-   `1 <= s.length <= 16`
    
-   `s`  contains only lower case English letters.

# Solution 1: BackTracking (Beat 100%)
```
class Solution {
    int max = 0;
    public int maxUniqueSplit(String s) {
        HashSet<String> set = new HashSet<String>();
        dfs(s, set, 0);0
        return max;
    }
    
    public void dfs(String s, HashSet<String> set, int index){
        if(index == s.length()){
            if(set.size() > max) max = set.size();
            return;
        }
        
        for(int i = index + 1; i <= s.length(); i++){
            String cur = s.substring(index, i);
            if(set.contains(cur)) continue;
            if(set.size() + s.length() - i < max) break;
            set.add(cur);
            dfs(s,set,i);
            set.remove(cur);
        }
    }
}
```