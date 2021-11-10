# 20. Valid Parentheses 921 1541
Given a string  `s`  containing just the characters  `'('`,  `')'`,  `'{'`,  `'}'`,  `'['`  and  `']'`, determine if the input string is valid.

An input string is valid if:

1.  Open brackets must be closed by the same type of brackets.
2.  Open brackets must be closed in the correct order.

**Example 1:**

**Input:** s = "()"
**Output:** true

**Example 2:**

**Input:** s = "()[]{}"
**Output:** true

**Example 3:**

**Input:** s = "(]"
**Output:** false

**Example 4:**

**Input:** s = "([)]"
**Output:** false

**Example 5:**

**Input:** s = "{[]}"
**Output:** true

**Constraints:**

-   `1 <= s.length <= 104`
-   `s`  consists of parentheses only  `'()[]{}'`.

# Solution: Using Stack (Beat 60%)
```
class Solution {
    public boolean isValid(String s) {
        if(s.length() == 0) return true;
        if(s.length()%2 == 1) return false;
        Deque<Character> stack = new ArrayDeque<Character>();
        
        for(int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            if(c == '{' || c == '(' || c =='[') stack.push(c);
            else{
                if(stack.isEmpty()) return false;
                char last = stack.pop();
                if(c == ')' && last != '(') return false;
                if(c == ']' && last != '[') return false;
                if(c == '}' && last != '{') return false;
            }
        }     
        return stack.isEmpty();
    }
}
```


# Solution: Using Array to Mock Stack (Beat 100%)
```
class Solution {
    public boolean isValid(String s) {
        if(s.length() == 0) return true;
        if(s.length()%2 == 1) return false;
        char[] stack = new char[s.length()];
        int index = 0;
        
        for(int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            if(c == '{' || c == '(' || c =='[') stack[index++] = c;
            else{
                if(index == 0) return false;
                char last = stack[--index];
                if(c == ')' && last != '(') return false;
                if(c == ']' && last != '[') return false;
                if(c == '}' && last != '{') return false;
            }
        }
        
        return index == 0;
    }
}
```