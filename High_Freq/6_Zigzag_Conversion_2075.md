# 6. Zigzag Conversion 2075
The string  `"PAYPALISHIRING"`  is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

P   A   H   N
A P L S I I G
Y   I   R

And then read line by line:  `"PAHNAPLSIIGYIR"`

Write the code that will take a string and make this conversion given a number of rows:

string convert(string s, int numRows);

**Example 1:**

**Input:** s = "PAYPALISHIRING", numRows = 3
**Output:** "PAHNAPLSIIGYIR"

**Example 2:**

**Input:** s = "PAYPALISHIRING", numRows = 4
**Output:** "PINALSIGYAHRPI"
**Explanation:**
P     I    N
A   L S  I G
Y A   H R
P     I

**Example 3:**

**Input:** s = "A", numRows = 1
**Output:** "A"

**Constraints:**

-   `1 <= s.length <= 1000`
-   `s`  consists of English letters (lower-case and upper-case),  `','`  and  `'.'`.
-   `1 <= numRows <= 1000`

# Solution:
```
class Solution {
    public String convert(String s, int rows) {
        if(rows == 1 || s.length() <= 1) return s;
        int n = s.length(), p = rows + rows - 2;
        StringBuilder sb = new StringBuilder();
        
        for(int i = 0; i < rows; i++){
            int start = 0;
            if(i == 0 || i == rows - 1){
                while(start + i < n){
                    sb.append(s.charAt(start+i));
                    start += p;
                }
            }else{
                while(start + i < n){
                    sb.append(s.charAt(start + i));
                    if(start + p - i < n) sb.append(s.charAt(start + p - i));
                    start += p;
                }
            }
        }
        
        return sb.toString();
    }
}
```