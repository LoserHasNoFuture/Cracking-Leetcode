# 727. Minimum Window Subsequence 792
Given strings  `s1`  and  `s2`, return  _the minimum contiguous substring part of_ `s1`_, so that_ `s2` _is a subsequence of the part_.

If there is no such window in  `s1`  that covers all characters in  `s2`, return the empty string  `""`. If there are multiple such minimum-length windows, return the one with the  **left-most starting index**.

**Example 1:**

**Input:** s1 = "abcdebdde", s2 = "bde"
**Output:** "bcde"
**Explanation:** 
"bcde" is the answer because it occurs before "bdde" which has the same length.
"deb" is not a smaller window because the elements of s2 in the window must occur in order.

**Example 2:**

**Input:** s1 = "jmeqksfrsdcmsiwvaovztaqenprpvnbstl", s2 = "u"
**Output:** ""

**Constraints:**

-   `1 <= s1.length <= 2 * 104`
-   `1 <= s2.length <= 100`
-   `s1`  and  `s2`  consist of lowercase English letters.

# Solution 1: Two Pointer, Similar to 792 (Beat 47%)
```
class Solution {
    String newstr = "";
    String reversedS2 = "";
    public String minWindow(String s1, String s2) {
        newstr = s1;
        reversedS2 = reverseString(s2);
        String res = find_target_str(s2);
        if(res.length() == 0) return res;
        // System.out.println(res);
        
        while(newstr.length() >= s2.length()){
            String tmp = find_target_str(s2);
            // System.out.println(tmp);
            if(tmp.length() == 0) break;
            if(tmp.length() < res.length()) res = tmp;
        }
        
        return res;
    }
    
    // find the left most, but not the shortest
    public String find_target_str(String s2){
        int last = 0, index2 = 0;
        while(last < newstr.length() && index2 < s2.length()){
            if(newstr.charAt(last) == s2.charAt(index2)) index2++;
            last++;
        }
        if(index2 < s2.length()) return "";
        
        String str1 = reverseString(newstr.substring(0,last));
        last = 0; index2 = 0;
        while(last < str1.length() && index2 < reversedS2.length()){
            if(str1.charAt(last) == reversedS2.charAt(index2)) index2++;
            last++;
        }
        
        newstr = newstr.substring(str1.length() - last + 1);
        return reverseString(str1.substring(0,last));
    }
    
    public String reverseString(String str){
        int start = 0, end = str.length()-1;
        char[] word = str.toCharArray();
        
        while(start < end){
            char tmp = word[start];
            word[start] = word[end];
            word[end] = tmp;
            start++;
            end--;
        }
        return new String(word);
    }
}
```

# Solution 2: Improve from Solution 1 (Beat 100%)
```
class Solution {
    public String minWindow(String S, String T) {
        int m = T.length(), n = S.length();
        int ptr1 = 0, ptr2 = 0, res_start = -1, res_end = n;
        
        while(ptr1 < n){
            if(S.charAt(ptr1) == T.charAt(ptr2)){
                ptr2++;
                if(ptr2 == m){
                    int end = ptr1 + 1;
                    
//                     find the start point of S
                    ptr2--;
                    while(ptr2 >= 0){
                        if(T.charAt(ptr2) == S.charAt(ptr1)) ptr2--;
                        ptr1--;
                    }
//                     add 1 to make ptr2 = 0, and ptr1 = the start point
                    int start = ++ptr1;
                    ptr2++;
                    
                    if(res_end - res_start > end - start){
                        res_start = start;
                        res_end = end;
                    }
                    
                }
            }
            ptr1++;
        }
        
        return res_start == -1? "":S.substring(res_start,res_end);
    }
}
```

# Solution 3: Dynamic Programming (Beat 15%)	
dp[i][j] record the **start position** of the subsequence
```
class Solution {
    
    //dp[i][j] record the start position of the subsequence
    public String minWindow(String s1, String s2) {
        int m = s1.length(), n = s2.length();
        int[][] dp = new int[n+1][m+1];
        
        for(int i = 0; i <= n; i++) Arrays.fill(dp[i],-1);
        
        for(int i = 0; i <= m; i++) dp[0][i] = i;
        
        for(int i = 1; i <= n; i++){
            for(int j = 1; j <= m; j++){
                if(s1.charAt(j-1) == s2.charAt(i-1)) dp[i][j] = dp[i-1][j-1];
                else dp[i][j] = dp[i][j-1];
            }
        }
        
        int start = -1, end = m;
        for(int i = 0; i <= m; i++){
            if(dp[n][i] != -1){
                int cur_end = i;
                
                if(cur_end - dp[n][i] < end - start){
                    start = dp[n][i];
                    end = cur_end;
                }
            }
        }
        
        return start == -1? "":s1.substring(start,end);
    }

}
```