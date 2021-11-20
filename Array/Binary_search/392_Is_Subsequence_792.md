# 392. Is Subsequence
Given a string  **s**  and a string  **t**, check if  **s**  is subsequence of  **t**.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie,  `"ace"`  is a subsequence of  `"abcde"`  while  `"aec"`  is not).

**Follow up:**  
If there are lots of incoming S, say S1, S2, ... , Sk where k >= 1B, and you want to check one by one to see if T has its subsequence. In this scenario, how would you change your code?

**Credits:**  
Special thanks to  [@pbrother](https://leetcode.com/pbrother/)  for adding this problem and creating all test cases.

**Example 1:**

**Input:** s = "abc", t = "ahbgdc"
**Output:** true

**Example 2:**

**Input:** s = "axc", t = "ahbgdc"
**Output:** false

# Solution 1: Using Two Pointers O(n)
```
class Solution {
    public boolean isSubsequence(String t, String s) {
        if(t.length() > s.length()) return false;
        
        int p1 = 0, p2 = 0;
        while(p1 < t.length() && p2 < s.length()){
            if(s.charAt(p2) == t.charAt(p1)) p1++;
            p2++;
        }
        
        if(p1 < t.length()) return false;
        else return true;        
    }
}
```

# Solution 2: Using HashMap (For follow-up)
O(n) for initializating map
O(mlogn) for each t, where m is the length of t
Using binary search:
```
class Solution {
    public boolean isSubsequence(String t, String s) {
        if(t.length() > s.length()) return false;
        
        List[] map = new ArrayList[256];
        for(int i = 0; i < 256; i++) map[i] = new ArrayList<Integer>();
        for(int i = 0; i < s.length(); i++) map[s.charAt(i)].add(i);
        
        
        int prev = -1;
        for(int i = 0; i < t.length(); i++){
            List<Integer> arr = map[t.charAt(i)];
            if(arr.size() == 0) return false;
            else{
                int start = 0;
                int end = arr.size() - 1;
                while(start < end){
                    int mid = (start + end) >>> 1;
                    if(arr.get(mid) <= prev) start = mid + 1;
                    else end = mid;
                }
                
                if(arr.get(start) > prev){
                    prev = arr.get(start);
                } else return false;
                
            }
        }
        return true;
        
    }
}
```
