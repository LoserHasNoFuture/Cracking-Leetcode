# 856. Score of Parentheses 1614, 1111, 1021
Given a balanced parentheses string  `s`, return  _the  **score**  of the string_.

The  **score**  of a balanced parentheses string is based on the following rule:

-   `"()"`  has score  `1`.
-   `AB`  has score  `A + B`, where  `A`  and  `B`  are balanced parentheses strings.
-   `(A)`  has score  `2 * A`, where  `A`  is a balanced parentheses string.

**Example 1:**

**Input:** s = "()"
**Output:** 1

**Example 2:**

**Input:** s = "(())"
**Output:** 2

**Example 3:**

**Input:** s = "()()"
**Output:** 2

**Example 4:**

**Input:** s = "(()(()))"
**Output:** 6

**Constraints:**

-   `2 <= s.length <= 50`
-   `s`  consists of only  `'('`  and  `')'`.
-   `s`  is a balanced parentheses string.


# Solution 1: Using Stack
```
class Solution {
    public int scoreOfParentheses(String s) {
        int n = s.length(), index = -1, res = 0;
        int[] stack = new int[n];
        
        for(int i = 0; i < n; i++){
            char c = s.charAt(i);
            if(c == ')' && index >= 0){
                int cnt = stack[index] == 0 ? 1:stack[index]*2;
                index--;
                if(index < 0) res += cnt;
                else stack[index] += cnt;
            }else stack[++index] = 0;
        } 
        
        return res;
    }
}
```

# Solution 2: O(n) Time, O(1) Space
```
class Solution {
    public int scoreOfParentheses(String s) {
        int n = s.length(), layer = 0, res = 0;
        
        for(int i = 0; i < n; i++){
            char c = s.charAt(i);
            if(c == '(') layer++;
            else layer--;
            
            if(c == ')' && s.charAt(i-1) == '(') res += (1 << layer);
        }
        
        return res;
    }
}
```
