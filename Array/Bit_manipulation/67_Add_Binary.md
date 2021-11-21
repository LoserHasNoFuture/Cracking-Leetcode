# 67. Add Binary
  
Given two binary strings, return their sum (also a binary string).

The input strings are both  **non-empty**  and contains only characters  `1`  or `0`.

**Example 1:**

**Input:** a = "11", b = "1"
**Output:** "100"

**Example 2:**

**Input:** a = "1010", b = "1011"
**Output:** "10101"

**Constraints:**

-   Each string consists only of  `'0'`  or  `'1'`  characters.
-   `1 <= a.length, b.length <= 10^4`
-   Each string is either  `"0"`  or doesn't contain any leading zero.

# Solution 1: Intuitive approach
```
class Solution {
    public String addBinary(String a, String b) {
        StringBuilder builder = new StringBuilder();
        
        int end1 = a.length() - 1;
        int end2 = b.length() - 1;
        
        int cin = 0;
        while(end1 >= 0 || end2 >= 0){
            int sum = cin;
            // A good way to convert char to number
            // using (int) a.charAt() is actually getting its ASCII code
            if(end1 >= 0) sum += a.charAt(end1--) - '0';
            if(end2 >= 0) sum += b.charAt(end2--) - '0';
            builder.append(sum%2);
            cin = sum/2;
        }
        
        if(cin != 0) builder.append(cin%2);

        return builder.reverse().toString();
    }
}
```