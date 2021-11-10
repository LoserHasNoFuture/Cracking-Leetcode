# 844. Backspace String Compare
Given two strings `S` and  `T`, return if they are equal when both are typed into empty text editors.  `#`  means a backspace character.

Note that after backspacing an empty text, the text will continue empty.

**Example 1:**

**Input:** S = "ab#c", T = "ad#c"
**Output:** true **Explanation**: Both S and T become "ac".

**Example 2:**

**Input:** S = "ab##", T = "c#d#"
**Output:** true **Explanation**: Both S and T become "".

**Example 3:**

**Input:** S = "a##c", T = "#a#c"
**Output:** true **Explanation**: Both S and T become "c".

**Example 4:**

**Input:** S = "a#c", T = "b"
**Output:** false **Explanation**: S becomes "c" while T becomes "b".

**Note**:

-   `1 <= S.length <= 200`
-   `1 <= T.length <= 200`
-   `S` and  `T`  only contain lowercase letters and  `'#'`  characters.

**Follow up:**

-   Can you solve it in  `O(N)`  time and  `O(1)`  space?

# Solution 1: Using Stack (Beat 36%)
O(n) Space, O(n) Time
```
class Solution {
    public boolean backspaceCompare(String S, String T) {
        return parse_string(S).equals(parse_string(T));
    }
    
    public String parse_string(String S){
        Deque<Character> stack = new ArrayDeque<Character>();
        
        for(char c : S.toCharArray()){
            if(c == '#'){
                if(!stack.isEmpty()) stack.pop();
            }else stack.push(c);
        }
        
        return stack.toString();
        
    }
}
```

# Solution 2: Comparing backwards (Beat 100%)
O(1) Space, O(n) Time
```
class Solution {
    public boolean backspaceCompare(String S, String T) {
        int ptr1 = S.length() -1, ptr2 = T.length() - 1;
        
        while(ptr1 >= 0 || ptr2 >= 0){
            ptr1 = find_next_char(S,ptr1);
            ptr2 = find_next_char(T,ptr2);
            
            if(ptr1 >= 0 && ptr2 >= 0){
                if(S.charAt(ptr1) != T.charAt(ptr2)) return false;
                else{
                    ptr1--; ptr2--;
                }
            }else if(ptr1 >= 0 || ptr2 >= 0) return false;
        }
        
        return true;
    }
    
    
    public int find_next_char(String s, int ptr){
        int cnt_s = 0;
        while(ptr >= 0 && s.charAt(ptr) == '#' || cnt_s > 0){
            if(ptr >= 0 && s.charAt(ptr) == '#') cnt_s++;
            else cnt_s--;
            ptr--;
        }    
        return ptr;
    }
}
```