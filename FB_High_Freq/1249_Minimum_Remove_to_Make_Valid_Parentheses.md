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
        StringBuilder sb = new StringBuilder();
        Deque<Integer> stack = new ArrayDeque<>();
        
        int start = 0, len = s.length();
        for(int i = 0; i < len; i++){
            char c = s.charAt(i);
            if(c == '(') stack.push(i);
            else if(c == ')'){
                if(stack.isEmpty()) {
                    sb.append(s.substring(start,i));
                    start = i + 1;
                }else stack.pop();
            }
        }
        
        LinkedList<Integer> li = new LinkedList<Integer>();
        while(!stack.isEmpty()) li.add(0,stack.pop());
        
        for(int pos: li){
            if(start >= len) break;
            sb.append(s.substring(start,pos));
            start = pos+1;
        }
        
        if(start < len) sb.append(s.substring(start,len));
        
        return sb.toString();
    }
}
```