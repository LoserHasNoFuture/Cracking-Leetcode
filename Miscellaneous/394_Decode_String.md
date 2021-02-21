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


# Solution 1: DFS Recursion(Beat 100%)
```
class Solution {
    public String decodeString(String s) {
        return dfs(s,0,s.length());
    }
    
    public String dfs(String s, int start, int end){
        StringBuilder sb = new StringBuilder();
        
        int index = start;
        while(index < end){
            if(s.charAt(index) >= 'a' && s.charAt(index) <= 'z') sb.append(s.charAt(index));
            else{
                int lb = s.indexOf('[',index);
                int cnt = Integer.parseInt(s.substring(index,lb));
                int rb = find_right_bracket(s,lb);
                String str = dfs(s,lb+1,rb);
                while(cnt-- > 0) sb.append(str);
                index = rb;
            }
            index++;
        }
        
        return sb.toString();
    }
    
    
    public int find_right_bracket(String s, int lb){
        int next_lb = s.indexOf('[',lb+1);
        int rb = s.indexOf(']',lb+1);
        
        while(next_lb != -1 && next_lb < rb){
            next_lb = s.indexOf('[',next_lb+1);
            rb = s.indexOf(']',rb+1);
        }
        
        return rb;
    }
}
```


# Solution 2: Using Stack (Beat 23%)
Refer from: [https://leetcode.com/problems/decode-string/discuss/87534/Simple-Java-Solution-using-Stack](https://leetcode.com/problems/decode-string/discuss/87534/Simple-Java-Solution-using-Stack)
```
public class Solution {
    public String decodeString(String s) {
        String res = "";
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
            }
            else if (s.charAt(idx) == '[') {
                resStack.push(res);
                res = "";
                idx++;
            }
            else if (s.charAt(idx) == ']') {
                StringBuilder temp = new StringBuilder (resStack.pop());
                int repeatTimes = countStack.pop();
                for (int i = 0; i < repeatTimes; i++) {
                    temp.append(res);
                }
                res = temp.toString();
                idx++;
            }
            else {
                res += s.charAt(idx++);
            }
        }
        return res;
    }
}
```