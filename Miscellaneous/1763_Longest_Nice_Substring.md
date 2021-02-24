# 1763. Longest Nice Substring
A string  `s`  is  **nice**  if, for every letter of the alphabet that  `s`  contains, it appears  **both**  in uppercase and lowercase. For example,  `"abABB"`  is nice because  `'A'`  and  `'a'`  appear, and  `'B'`  and  `'b'`  appear. However,  `"abA"`  is not because  `'b'`  appears, but  `'B'`  does not.

Given a string  `s`, return  _the longest  **substring**  of  `s`  that is  **nice**. If there are multiple, return the substring of the  **earliest**  occurrence. If there are none, return an empty string_.

**Example 1:**

**Input:** s = "YazaAay"
**Output:** "aAa"
**Explanation:** "aAa" is a nice string because 'A/a' is the only letter of the alphabet in s, and both 'A' and 'a' appear.
"aAa" is the longest nice substring.

**Example 2:**

**Input:** s = "Bb"
**Output:** "Bb"
**Explanation:** "Bb" is a nice string because both 'B' and 'b' appear. The whole string is a substring.

**Example 3:**

**Input:** s = "c"
**Output:** ""
**Explanation:** There are no nice substrings.

**Example 4:**

**Input:** s = "dDzeE"
**Output:** "dD"
**Explanation:** Both "dD" and "eE" are the longest nice substrings.
As there are multiple longest nice substrings, return "dD" since it occurs earlier.

**Constraints:**

-   `1 <= s.length <= 100`
-   `s`  consists of uppercase and lowercase English letters.

# Solution 1: Brute Force (Beat 90%)
```
class Solution {
    public String longestNiceSubstring(String s) {
        if(s.length() <= 1) return "";
        
        String res = "";
        
        int start = 0;
        while(start < s.length()){
            int end = -1;
            if(s.charAt(start) - 'a' >= 0) end = s.indexOf((char)(s.charAt(start) - 32), start+1);
            else end = s.indexOf((char)(s.charAt(start) + 32), start+1);
            
            if(end > 0){
                String str = find_longest_starting_from(start, end, s);
                if(str.length() > res.length()) res = str;
            }
            start++;
        }
        
        return res;
    }
    
    public String find_longest_starting_from(int start, int end, String s){
        
        int cnt = 0, res_end = -1;
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
            
        for(int i = start; i < s.length(); i++){
            if(s.charAt(i) - 'a' >= 0){
                int val = map.getOrDefault(s.charAt(i), 0);
                if(val == 0) {
                    val++;
                    cnt++;
                }else if(val == -1) {
                    val = 10;
                    cnt--;
                }
                
                map.put(s.charAt(i),val);
            }else{
                int val = map.getOrDefault((char)(s.charAt(i)+32), 0);
                if(val == 0) {
                    val--;
                    cnt++;
                }
                else if(val == 1){
                    val = 10; cnt--;
                }
                 
                map.put((char)(s.charAt(i)+32),val);
            }
            
            if(i >= end && cnt == 0) res_end = i;
        }
        if(res_end == -1) return new String();
        else return s.substring(start,res_end+1);   
    } 
}
```

# Solution 2: Divide and Conquer  (Beat 100%)
Refer From: [https://leetcode.com/problems/longest-nice-substring/discuss/1074589/JavaStraightforward-Divide-and-Conquer](https://leetcode.com/problems/longest-nice-substring/discuss/1074589/JavaStraightforward-Divide-and-Conquer)
```
class Solution {
    public String longestNiceSubstring(String s) {
        if (s.length() < 2) return "";
        char[] arr = s.toCharArray();
        Set<Character> set = new HashSet<>();
        for (char c: arr) set.add(c);
        for (int i = 0; i < arr.length; i++) {
            char c = arr[i];
            if (set.contains(Character.toUpperCase(c)) && set.contains(Character.toLowerCase(c))) continue;
            String sub1 = longestNiceSubstring(s.substring(0, i));
            String sub2 = longestNiceSubstring(s.substring(i+1));
            return sub1.length() >= sub2.length() ? sub1 : sub2;
        }
        return s; 
    }
}

```