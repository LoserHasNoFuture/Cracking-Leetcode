# 1415. The k-th Lexicographical String of All Happy Strings of Length n 60
A  **happy string**  is a string that:

-   consists only of letters of the set  `['a', 'b', 'c']`.
-   `s[i] != s[i + 1]` for all values of  `i`  from  `1`  to  `s.length - 1`  (string is 1-indexed).

For example, strings  **"abc", "ac", "b"**  and  **"abcbabcbcb"**  are all happy strings and strings  **"aa", "baa"**  and **"ababbc"**  are not happy strings.

Given two integers  `n`  and  `k`, consider a list of all happy strings of length  `n`  sorted in lexicographical order.

Return  _the kth string_  of this list or return an  **empty string** if there are less than  `k`  happy strings of length  `n`.

**Example 1:**

**Input:** n = 1, k = 3
**Output:** "c"
**Explanation:** The list ["a", "b", "c"] contains all happy strings of length 1. The third string is "c".

**Example 2:**

**Input:** n = 1, k = 4
**Output:** ""
**Explanation:** There are only 3 happy strings of length 1.

**Example 3:**

**Input:** n = 3, k = 9
**Output:** "cab"
**Explanation:** There are 12 different happy string of length 3 ["aba", "abc", "aca", "acb", "bab", "bac", "bca", "bcb", "cab", "cac", "cba", "cbc"]. You will find the 9th string = "cab"

**Example 4:**

**Input:** n = 2, k = 7
**Output:** ""

**Example 5:**

**Input:** n = 10, k = 100
**Output:** "abacbabacb"

**Constraints:**

-   `1 <= n <= 10`
-   `1 <= k <= 100`

# Solution 1: BackTracking (Beat 98%)
```
class Solution {
    
    int cnt;
    public String getHappyString(int n, int k) {
        StringBuilder sb = new StringBuilder();
        cnt = k;
        dfs(n, -1, 0,sb);
        return sb.reverse().toString();
    }
    
    public boolean dfs(int n, int index, int depth, StringBuilder sb){
        if(cnt < 0) return false;
        
        if(depth == n && --cnt == 0) return true;
        
        for(int i = 0;  i < 3; i++){
            if(cnt > 0 && depth < n && index != i){
                if(dfs(n,i,depth+1,sb)){
                    sb.append((char)(97+i));
                    return true;
                }
            }
        }
        
        return false;
    }
}
```

# Solution 2: BackTracking + A Little Math
Follow Up:
Extend letters set to  `['a', 'b', 'c', ..., 'z']`.
```
class Solution {
    
    int cnt;
    public String getHappyString(int n, int k) {
        StringBuilder sb = new StringBuilder();
        cnt = k;
        dfs(n, -1, 0,sb);
        return sb.reverse().toString();
    }
    
    public int dfs(int n, int index, int depth, StringBuilder sb){
        if(depth == n){
            cnt--;
            return 1;
        } 
        
        int tmp = -1;
        for(int i = 0;  i < 3; i++){
            if(cnt > 0 && depth < n && index != i){
                if(tmp < 0) {
                    tmp = dfs(n,i,depth+1,sb);
                    if(cnt < 0){
                        i = i + 2; 
                        cnt += tmp;
                        continue;
                    }else if(cnt == 0){
                        sb.append((char)(97+i));
                        return tmp*2;
                    }
                }else{
                    if(cnt - tmp > 0) cnt -= tmp;
                    else{
                        dfs(n,i,depth+1,sb);
                        if(cnt == 0){
                            sb.append((char)(97+i));
                            return tmp*2;
                        }
                    }
                }
                
            }
        }
        
        return tmp * 2;
    }
}
```