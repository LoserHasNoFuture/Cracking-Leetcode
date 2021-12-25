# 224 227 772 . Basic Calculator
Implement a basic calculator to evaluate a simple expression string.

The expression string contains only non-negative integers,  `'+'`,  `'-'`,  `'*'`,  `'/'`  operators, and open  `'('`  and closing parentheses  `')'`. The integer division should  **truncate toward zero**.

You may assume that the given expression is always valid. All intermediate results will be in the range of  `[-231, 231  - 1]`.

**Note:**  You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as  `eval()`.

**Example 1:**

**Input:** s = "1+1"
**Output:** 2

**Example 2:**

**Input:** s = "6-4/2"
**Output:** 4

**Example 3:**

**Input:** s = "2*(5+5*2)/3+(6/2+8)"
**Output:** 21

**Constraints:**

-   `1 <= s <= 104`
-   `s`  consists of digits,  `'+'`,  `'-'`,  `'*'`,  `'/'`,  `'('`, and `')'`.
-   `s`  is a  **valid**  expression.

# Solution: Stack + Recursion
```
class Solution {
    int ptr = 0;
    public int calculate(String s) {
        int num = 0, n = s.length();
        char operation = '+';
        Deque<Integer> stack = new ArrayDeque<Integer>();
        while(ptr < n){
            char c = s.charAt(ptr++);
            if(c >= '0' && c <= '9') num = num*10 + (c-'0');
            
            if(c == '(') num = calculate(s);
            if(c == '+' || c == '-' || c == '*' || c == '/'){
                if(operation == '+') stack.push(num);
                if(operation == '-') stack.push(-num);
                if(operation == '*') stack.push(num*stack.pop());
                if(operation == '/') stack.push(stack.pop()/num);
                operation = c;
                num = 0;
            }
            if(c == ')') break;
        }
        
        int res = 0;
        if(operation == '+') stack.push(num);
        if(operation == '-') stack.push(-num);
        if(operation == '*') stack.push(num*stack.pop());
        if(operation == '/') stack.push(stack.pop()/num);
        while(!stack.isEmpty()) res += stack.pop();
        return res;
    }
}
```