# 1249. Minimum Remove to Make Valid Parentheses
Given a string  s of `'('` , `')'` and lowercase English characters.

Your task is to remove the minimum number of parentheses ( `'('` or `')'`, in any positions ) so that the resulting  _parentheses string_  is valid and return  **any**  valid string.

Formally, a  _parentheses string_  is valid if and only if:

-   It is the empty string, contains only lowercase characters, or
-   It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
-   It can be written as `(A)`, where `A` is a valid string.

**Example 1:**

**Input:** s = "lee(t(c)o)de)"
**Output:** "lee(t(c)o)de"
**Explanation:** "lee(t(co)de)" , "lee(t(c)ode)" would also be accepted.

**Example 2:**

**Input:** s = "a)b(c)d"
**Output:** "ab(c)d"

**Example 3:**

**Input:** s = "))(("
**Output:** ""
**Explanation:** An empty string is also valid.

**Example 4:**

**Input:** s = "(a(b(c)d)"
**Output:** "a(b(c)d)"

**Constraints:**

-   `1 <= s.length <= 10^5`
-   `s[i]` is one of `'('`  ,  `')'`  and lowercase English letters`.`

# Solution 1: Using Stack (Beat 95%)
```
class Solution {
    public String minRemoveToMakeValid(String s) {
        int n = s.length(), index = -1;
        int[] stack = new int[n];
        
        for(int i = 0; i < n; i++){
            char c = s.charAt(i);
            if(c != '(' && c != ')') continue;
            if(c == ')' && index >= 0 && s.charAt(stack[index]) == '(') index--;
            else stack[++index] = i;
        }
        
        StringBuilder sb = new StringBuilder();
        int j = 0;
        for(int i = 0; i < n; i++){
            if(j <= index && stack[j] == i) j++;
            else sb.append(s.charAt(i));
        }
        
        return sb.toString();
    }
}
```

# Solution 2: O(n) Space and O(n) Time
```
class Solution {
    public String minRemoveToMakeValid(String s) {
        int n = s.length(), open = 0, close = 0;
        StringBuilder sb = new StringBuilder();
        
        for(int i = 0; i < n; i++){
            char c = s.charAt(i);
            if(c == '(') open++;
            else if(c == ')'){
                if(open == 0) continue;
                open--;
            }
            sb.append(c);
        }
        
        s = sb.toString();
        sb.setLength(0);
        for(int i = s.length()-1; i >= 0; i--){
            char c = s.charAt(i);
            if(c == ')') close++;
            else if(c == '('){
                if(close == 0) continue;
                close--;
            }
            sb.append(c);
        }
        
        return sb.reverse().toString();
    }
}
```

# Solution 3: Evolve from Solution 2, Using O(1) Space
```
class Solution {
    public String minRemoveToMakeValid(String s) {
        int n = s.length(), open = 0, close = 0;
        StringBuilder sb = new StringBuilder();
        
        for(int i = 0; i < n; i++){
            char c = s.charAt(i);
            if(c == ')') close++;
        }
        
        for(int i = 0; i < n; i++){
            char c = s.charAt(i);
            if(c == '('){
                if(close > 0){
                    open++;
                    close--;
                }else continue;
            }else if(c == ')'){
                if(open > 0) open--;
                else {
                    close--;
                    continue;
                }
            }
            sb.append(c);
        }
        
        return sb.toString();
    }
}
```