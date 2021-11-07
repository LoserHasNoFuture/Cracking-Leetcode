# 43. Multiply Strings
Given two non-negative integers  `num1`  and  `num2`  represented as strings, return the product of  `num1`  and  `num2`, also represented as a string.

**Note:** You must not use any built-in BigInteger library or convert the inputs to integer directly.

**Example 1:**

**Input:** num1 = "2", num2 = "3"
**Output:** "6"

**Example 2:**

**Input:** num1 = "123", num2 = "456"
**Output:** "56088"

**Constraints:**

-   `1 <= num1.length, num2.length <= 200`
-   `num1`  and  `num2`  consist of digits only.
-   Both  `num1`  and  `num2` do not contain any leading zero, except the number  `0`  itself.

# Solution: Product Property
Refer from: [https://leetcode.com/problems/multiply-strings/discuss/17608/AC-solution-in-Java-with-explanation](https://leetcode.com/problems/multiply-strings/discuss/17608/AC-solution-in-Java-with-explanation)
```
class Solution {
    public String multiply(String num1, String num2) {
        if(num1.equals("0") || num2.equals("0")) return "0";
        if(num1.equals("1")) return num2;
        if(num2.equals("1")) return num1;
        StringBuilder sb = new StringBuilder();
        int cin = 0, n1 = num1.length(), n2 = num2.length();
        int[] product = new int[n1+n2];
        
		// KEY!!!
        for(int i = n1-1; i >= 0; i--){
            for(int j = n2-1; j>= 0; j--){
                int x = num1.charAt(i) - '0';
                int y = num2.charAt(j) - '0';
                product[i+j+1] += (x*y);
            }
        }
        
        for(int i = n1 + n2 - 1; i >= 0; i--){
            product[i] += cin;
            cin = product[i]/10;
            product[i] = product[i]%10;
        }
        
        for(int i = 0; i < n1 + n2; i++) sb.append(product[i]);
        while (sb.length() != 0 && sb.charAt(0) == '0') sb.deleteCharAt(0);
        return sb.toString();
    }
}
```