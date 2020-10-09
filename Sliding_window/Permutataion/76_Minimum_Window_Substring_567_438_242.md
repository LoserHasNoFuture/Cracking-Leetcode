# 76. Minimum Window Substring 567 438 242
Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

**Example:**

**Input: S** = "ADOBECODEBANC", **T** = "ABC"
**Output:** "BANC"

**Note:**

-   If there is no such window in S that covers all characters in T, return the empty string  `""`.
-   If there is such window, you are guaranteed that there will always be only one unique minimum window in S.


# Solution: Sliding Window + Permutation
Easier but with more time complexity:
```
class Solution {
    public String minWindow(String s, String t) {
        if(t.length() == 0) return new String();
        char[] s_array = s.toCharArray();
        char[] t_array = t.toCharArray();
    
        int[] map = new int[256];
        for(int i = 0; i < t_array.length; i++) map[t_array[i]] ++;
        
        int min = Integer.MAX_VALUE;
        int res_start = 0;
        int res_end = 0;
        
        int start = 0;
        while(start < s_array.length && map[s_array[start]] == 0) start++;
        int end = start;
        
        while(end < s.length()){
            char c = s_array[end];
            map[c]--;
            if(map[c] == 0  && NoGreaterThanZero(map)){
                while(start <= end){
                    map[s_array[start]] = map[s_array[start]] + 1;
                    if(map[s_array[start]] > 0){
                        if(min > end-start+1){
                            min = end-start+1;
                            res_start = start;
                            res_end = end+1;   
                        }
                        start++;
                        break;
                    }
                    start++;
                }
            }

            end++;
        }
        
        return min == Integer.MAX_VALUE?new String():s.substring(res_start,res_end);
    }
    
    
    public boolean NoGreaterThanZero(int[] map){
        for(int count:map){
            if(count>0) return false;
        }
        return true;
    }
}
```

The complicated but better way:
```
class Solution {
    public String minWindow(String s, String t) {
        char[] s_array = s.toCharArray();
        char[] t_array = t.toCharArray();
        
        int[] map = new int[256];
        for(int i = 0; i < t_array.length; i++)
            map[t_array[i]] ++;
        
        int min = Integer.MAX_VALUE;
        int res_start = 0; int res_end = 0;
        
        int end = 0;int start = 0;
        int unmatched = t.length();
        while(end < s_array.length){
            if(map[s_array[end]] > 0) unmatched--;
            map[s_array[end]]--;
            
            while(unmatched == 0){
                if(min > end - start + 1){
                    min = end - start + 1;
                    if(min == t.length()) return s.substring(start,end+1);
                    res_start = start;
                    res_end = end + 1;
                }
                
                if(map[s_array[start]] >= 0) unmatched++;
                map[s_array[start]]++;
                start++;
            }
            end++;
        }
        
        if(min == Integer.MAX_VALUE) return new String();
        return s.substring(res_start,res_end);
    }
}
```