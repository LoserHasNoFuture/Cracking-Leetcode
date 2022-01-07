# 241. Different Ways to Add Parentheses
Given a string  `expression`  of numbers and operators, return  _all possible results from computing all the different possible ways to group numbers and operators_. You may return the answer in  **any order**.

**Example 1:**

**Input:** expression = "2-1-1"
**Output:** [0,2]
**Explanation:**
((2-1)-1) = 0 
(2-(1-1)) = 2

**Example 2:**

**Input:** expression = "2*3-4*5"
**Output:** [-34,-14,-10,-10,10]
**Explanation:**
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10

**Constraints:**

-   `1 <= expression.length <= 20`
-   `expression`  consists of digits and the operator  `'+'`,  `'-'`, and  `'*'`.
-   All the integer values in the input expression are in the range  `[0, 99]`.


# Solution 1: Recursion
```
class Solution {
    public List<Integer> diffWaysToCompute(String str) {
        List<Integer> res = new ArrayList<Integer>();
        int ptr = 0, n = str.length();
        if(str == null || n == 0) return res;
        
        
        while(ptr < n){
            char c = str.charAt(ptr);
            if(!Character.isDigit(c)){
        
                List<Integer> left = diffWaysToCompute(str.substring(0, ptr));
                List<Integer> right = diffWaysToCompute(str.substring(ptr+1));
                
                for(int l: left){
                    for(int r: right){
                        if(c == '+') res.add(l+r);
                        else if(c == '-') res.add(l-r);
                        else if(c == '*') res.add(l*r);
                        else res.add(l/r);
                    }
                }
                
            }
            ptr++;
        }
        
        if(res.size() == 0) res.add(Integer.valueOf(str));
        
        return res;
    }
}
```

# Solution 2: DP
```
class Solution {
    public List<Integer> diffWaysToCompute(String str) {
        List<Integer> res = new ArrayList<Integer>();
        int ptr = 0, n = str.length();
        if(str == null || n == 0) return res;
        
        List<String> all = new ArrayList<String>();
        for(int i = 0; i < n; i++){
            int cnt = 0;
            while(i + cnt < n && Character.isDigit(str.charAt(i+cnt))) cnt++;
            all.add(str.substring(i, i+cnt));
            if(i+cnt < n) all.add(str.substring(i+cnt, i+cnt+1));
            i += cnt;
        }
        
        n = all.size()/2 + 1;
        ArrayList<Integer>[][] dp = new ArrayList[n][n];
        for(int i = 0; i < n; i++){
            dp[i][i] = new ArrayList<Integer>();            
            dp[i][i].add(Integer.valueOf(all.get(i*2)));
        }
        
        for(int len = 1; len < n; len++){
            for(int i = 0; i + len < n; i++){
                dp[i][i+len] = new ArrayList<Integer>();
                for(int j = i; j < i+len; j++){
                    ArrayList<Integer> left = dp[i][j];
                    ArrayList<Integer> right = dp[j+1][i+len];
                    char c = all.get(j*2 + 1).charAt(0);
                    
                    // System.out.println(left.size());
                    // System.out.println(right.size());
                    // System.out.println(c);
                    
                    for(int l: left){
                        for(int r: right){
                            if(c == '+') dp[i][i+len].add(l+r);
                            else if(c == '-') dp[i][i+len].add(l-r);
                            else if(c == '*') dp[i][i+len].add(l*r);
                            else dp[i][i+len].add(l/r);
                        }
                    }
                }
            }
        }
        
        return dp[0][n-1];
    }
}
```