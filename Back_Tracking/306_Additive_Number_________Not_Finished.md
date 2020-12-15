# 306. Additive Number  (Not Finished)
Additive number is a string whose digits can form additive sequence.

A valid additive sequence should contain  **at least**  three numbers. Except for the first two numbers, each subsequent number in the sequence must be the sum of the preceding two.

Given a string containing only digits  `'0'-'9'`, write a function to determine if it's an additive number.

**Note:**  Numbers in the additive sequence  **cannot**  have leading zeros, so sequence  `1, 2, 03`  or  `1, 02, 3`  is invalid.

**Example 1:**

**Input:** "112358"
**Output:** true
**Explanation:** The digits can form an additive sequence: 1, 1, 2, 3, 5, 8. 
             1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8

**Example 2:**

**Input:** "199100199"
**Output:** true
**Explanation:** The additive sequence is: 1, 99, 100, 199. 
             1 + 99 = 100, 99 + 100 = 199

**Constraints:**

-   `num` consists only of digits  `'0'-'9'`.
-   `1 <= num.length <= 35`

**Follow up:**    (Not done)
How would you handle overflow for very large input integers?


# Solution 1: BackTracking (Beat 27.55%)
```
class Solution {
    public boolean isAdditiveNumber(String s) {
        if(s.length() < 3) return false;        
        return dfs(s, -1, -1, 0);
    }
    
    public boolean dfs(String s, int num1, int num2, int index){
        if(index >= s.length()) return true;
        
        int num = 0;
        while(index < s.length()){
            num = num*10 + Integer.parseInt(s.charAt(index++)+"");
            
            if(num1 != -1 && num2 != -1 && num == num1 + num2) {
                if(dfs(s, num2, num, index)) return true;
            }
            if(num1 == -1){
                if(index < s.length()  && dfs(s, num, -1, index)) return true;
            } else if(num2 == -1 && index < s.length() && dfs(s, num1, num, index)) return true;
            
            if(num == 0) return false;
        }
        
        return false;
    }
}
```