# 394. Decode String
Given an encoded string, return its decoded string.

The encoding rule is:  `k[encoded_string]`, where the  `encoded_string`  inside the square brackets is being repeated exactly  `k`  times. Note that  `k`  is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers,  `k`. For example, there won't be input like  `3a`  or  `2[4]`.

**Example 1:**

**Input:** s = "3[a]2[bc]"
**Output:** "aaabcbc"

**Example 2:**

**Input:** s = "3[a2[c]]"
**Output:** "accaccacc"

**Example 3:**

**Input:** s = "2[abc]3[cd]ef"
**Output:** "abcabccdcdcdef"

**Example 4:**

**Input:** s = "abc3[cd]xyz"
**Output:** "abccdcdcdxyz"

**Constraints:**

-   `1 <= s.length <= 30`
-   `s`  consists of lowercase English letters, digits, and square brackets  `'[]'`.
-   `s`  is guaranteed to be  **a valid**  input.
-   All the integers in  `s`  are in the range  `[1, 300]`.

# Solution 1: Recursion
```
class Solution {
    int index = 0;
    public String decodeString(String s) {
        StringBuilder sb = new StringBuilder();
        int cnt = 0;
        while(index < s.length()){
            char c = s.charAt(index++);
            if(c >= 'a' && c <= 'z') sb.append(c+"");
            else if(c >= '0' && c <= '9') cnt = cnt*10 + (c-'0');
            else if(c == '['){
                String str = decodeString(s);
                for(int i = 0; i < cnt; i++) sb.append(str);
                
                cnt = 0;
            }else break;
        }
        
        return sb.toString();
    }
    
    
}
```

# Solution 2: Iterative, Use Stack
需要两个stack, 分别记录前面的数字和字符串
```
public class Solution {
    public String decodeString(String s) {
        StringBuilder res = new StringBuilder();
        Stack<Integer> countStack = new Stack<>();
        Stack<String> resStack = new Stack<>();
        int idx = 0;
        while (idx < s.length()) {
            if (Character.isDigit(s.charAt(idx))) {
                int count = 0;
                while (Character.isDigit(s.charAt(idx))) {
                    count = 10 * count + (s.charAt(idx) - '0');
                    idx++;
                }
                countStack.push(count);
            }else if (s.charAt(idx) == '[') {
                resStack.push(res.toString());
                res = new StringBuilder();
                idx++;
            }
            else if (s.charAt(idx) == ']') {
                StringBuilder temp = new StringBuilder (resStack.pop());
                int repeatTimes = countStack.pop();
                for (int i = 0; i < repeatTimes; i++) {
                    temp.append(res.toString());
                }
                res = temp;
                idx++;
            }
            else {
                res.append(s.charAt(idx++));
            }
        }
        return res.toString();
    }
}
```