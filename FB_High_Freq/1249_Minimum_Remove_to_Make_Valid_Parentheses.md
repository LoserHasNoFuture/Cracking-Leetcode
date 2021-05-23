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

# Solution 2: Constant Space Algo (Beat 85%)
Refer from: [https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/discuss/419466/Constant-Space-Solution](https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/discuss/419466/Constant-Space-Solution)

Key idea: count the number of open and close parenthesis at each position
```
class Solution {
    public String minRemoveToMakeValid(String s) {
        int open = 0, close = 0;
        StringBuilder sb = new StringBuilder();
        
        // total # of )
        for(char c: s.toCharArray()){
            if(c == ')') close++;
        }
        
        for(char c: s.toCharArray()){
            if(c == '('){
                open++;
                if(close > 0){
                // we can find a ) for this (
                    sb.append(c);
                    close--;
                }
            }else if(c == ')'){
                if(open > 0){
                // this ) can be matched with a previous (
                    open--;
                    sb.append(c);
                }else{
                // this ) can not be matched with any future (
                    close--;
                }
            }else sb.append(c);
            
        }
        
        return sb.toString();
    }
}
```

# Solution 3: Another Lamer Constant Space Algo (Beat 18%)
From Zhengqi.
```
class Solution {
    public String minRemoveToMakeValid(String s) {
        int open = 0;
        StringBuilder sb = new StringBuilder();
        
//         remove extra )
        for(char c: s.toCharArray()){
            if(c == '(') open++;
            else if(c == ')'){
                if(open == 0) continue;
                else open--;
            }
            sb.append(c);
        }

//         remove extra (        
        s = sb.toString(); sb.setLength(0);
        int close = 0;
        for(int i = s.length() - 1; i >= 0; i--){
            char c = s.charAt(i);
            if(c == ')') close++;
            else if(c == '('){
                if(close == 0) continue;
                else close--;
            }
            sb.append(c);
        }
        
        return sb.reverse().toString();
    }
}
```
