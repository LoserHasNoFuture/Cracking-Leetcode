# 1541. Minimum Insertions to Balance a Parentheses String 20 921
Given a parentheses string  `s`  containing only the characters  `'('`  and  `')'`. A parentheses string is  **balanced**  if:

-   Any left parenthesis `'('` must have a corresponding two consecutive right parenthesis `'))'`.
-   Left parenthesis `'('` must go before the corresponding two consecutive right parenthesis `'))'`.

In other words, we treat  `'('`  as openning parenthesis and  `'))'`  as closing parenthesis.

For example,  `"())"`,  `"())(())))"`  and  `"(())())))"`  are balanced,  `")()"`,  `"()))"`  and  `"(()))"`  are not balanced.

You can insert the characters  `'('`  and  `')'`  at any position of the string to balance it if needed.

Return  _the minimum number of insertions_  needed to make  `s`  balanced.

**Example 1:**

**Input:** s = "(()))"
**Output:** 1
**Explanation:** The second '(' has two matching '))', but the first '(' has only ')' matching. We need to to add one more ')' at the end of the string to be "(())))" which is balanced.

**Example 2:**

**Input:** s = "())"
**Output:** 0
**Explanation:** The string is already balanced.

**Example 3:**

**Input:** s = "))())("
**Output:** 3
**Explanation:** Add '(' to match the first '))', Add '))' to match the last '('.

**Example 4:**

**Input:** s = "(((((("
**Output:** 12
**Explanation:** Add 12 ')' to balance the string.

**Example 5:**

**Input:** s = ")))))))"
**Output:** 5
**Explanation:** Add 4 '(' at the beginning of the string and one ')' at the end. The string becomes "(((())))))))".

**Constraints:**

-   `1 <= s.length <= 10^5`
-   `s`  consists of  `'('`  and  `')'`  only.

# Solution 1: Using Stack 
Using Stack to record the # of the `')'` needed.
```
class Solution {
    public int minInsertions(String s) {
        int res = 0, n = s.length(), index = -1;
        int[] stack = new int[n];
        
        for(int i = 0; i < n; i++){
            char c = s.charAt(i);
            if(c == '('){
                if(index < 0 || stack[index] == 2) stack[++index] = 2;
                else{
                    res++;
                    stack[index] = 2;
                }
            }else{
                if(index < 0){
                    res++;
                    stack[++index] = 1;
                }else if(stack[index] == 1) index--;
                else stack[index] = 1;
            }
        }
        
        for(int i = 0; i <= index; i++) res += stack[i];
        
        return res;
    }
    
}
```

# Solution 2: Count the number of `'('` and `')'`

```
class Solution {
    public int minInsertions(String s) {
        int res = 0, left = 0, right = 0;
        
        for(int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            if(c == '('){
                if(right%2 != 0) res++;
                left -= (right+1)/2;
                if(left < 0) {
                    res -= left;
                    left = 0;
                }
                right = 0;
                
                left++;
            }else right++;
        }
        
        
        if(right%2 != 0) res++;
        left -= (right+1)/2;
        if(left < 0) res -= left;
        else res += left*2;
        
        return res;
    }
}
```

```
class Solution {
    public int minInsertions(String s) {
        int res = 0;
        int left = 0;
        
        for(int i = 0; i < s.length(); i++){
            if(s.charAt(i) == '(') {
                if(left < 0){
                    res += make_even(left);
                    left = 0;
                }
                if(left%2 == 1) {
                    res++;
                    left--;
                }
                left = left+2;
            }else left--;
            
        }
        if(left < 0){
            res += make_even(left);
            left = 0;
        }
        return res + left;
    }
    
    public int make_even(int left){
        int res = (-left)/2;
        if((-left)%2 == 1) res = res+2;
        return res;
    }
}
```
Lee's clean code: [https://leetcode.com/problems/minimum-insertions-to-balance-a-parentheses-string/discuss/780199/JavaC%2B%2BPython-Straight-Forward-One-Pass](https://leetcode.com/problems/minimum-insertions-to-balance-a-parentheses-string/discuss/780199/JavaC%2B%2BPython-Straight-Forward-One-Pass)
```
class Solution {
    public int minInsertions(String s) {
        int res = 0, right = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) == '(') {
                if (right % 2 > 0) {
                    right--;
                    res++;
                }
                right += 2;
            } else {
                right--;
                if (right < 0) {
                    right += 2;
                    res++;
                }
            }
        }
        return right + res;
    }
}
```


