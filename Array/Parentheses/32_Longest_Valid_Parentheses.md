#  32. Longest Valid Parentheses
Given a string containing just the characters  `'('`  and  `')'`, find the length of the longest valid (well-formed) parentheses substring.

**Example 1:**

**Input:** s = "(()"
**Output:** 2
**Explanation:** The longest valid parentheses substring is "()".

**Example 2:**

**Input:** s = ")()())"
**Output:** 4
**Explanation:** The longest valid parentheses substring is "()()".

**Example 3:**

**Input:** s = ""
**Output:** 0

**Constraints:**

-   `0 <= s.length <= 3 * 104`
-   `s[i]`  is  `'('`, or  `')'`.

# Solution: Stack
```
class Solution {
    public int longestValidParentheses(String s) {
        Deque<Integer> stack = new ArrayDeque<Integer>();
        int n = s.length(), res = 0;
        
        for(int i = 0; i < n; i++){
            char c = s.charAt(i);
            if(c == ')' && !stack.isEmpty() && s.charAt(stack.peek()) == '('){
                stack.pop();
                res = Math.max(res, stack.isEmpty()?i+1: i - stack.peek());

            }else stack.push(i);
        }
        
        return res;
    }
}
```

# Solution: DP
```
class Solution {
    public int longestValidParentheses(String s) {
        int n = s.length(), res = 0;
        int[] dp = new int[n];
        
        for(int i = 0; i < n; i++){
            char c = s.charAt(i);
            if(c == '(') dp[i] = 0;
            else if(i > 0 && s.charAt(i-1) == '(') dp[i] = i > 1? dp[i-2]+2:2;
            else if(i > 0 && i-dp[i-1] -1 >= 0 
                   && s.charAt(i-dp[i-1] -1)  == '('){
                dp[i] = (dp[i-1] + 2) + (i-dp[i-1]-2 > 0? dp[i-dp[i-1]-2] : 0);
            }
            res = Math.max(res,dp[i]);
        }
        
        return res;
    }
}
```