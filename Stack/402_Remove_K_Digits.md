# 402. Remove K Digits
Given string num representing a non-negative integer  `num`, and an integer  `k`, return  _the smallest possible integer after removing_  `k`  _digits from_  `num`.

**Example 1:**

**Input:** num = "1432219", k = 3
**Output:** "1219"
**Explanation:** Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.

**Example 2:**

**Input:** num = "10200", k = 1
**Output:** "200"
**Explanation:** Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.

**Example 3:**

**Input:** num = "10", k = 2
**Output:** "0"
**Explanation:** Remove all the digits from the number and it is left with nothing which is 0.

**Constraints:**

-   `1 <= k <= num.length <= 105`
-   `num`  consists of only digits.
-   `num`  does not have any leading zeros except for the zero itself.


# Solution 1: Montonic Increasing Stack
```
class Solution {
    public String removeKdigits(String num, int k) {
        int n = num.length(), index = -1;
        if(n == k) return new String("0");
        char[] stack = new char[n];
        
        for(int i = 0; i < n; i++){
            char cur = num.charAt(i);
            while(index >= 0 && stack[index] > cur && k > 0){
                k--;
                index--;
            }
            if(cur == '0' && index == -1) continue;
            stack[++index] = cur;
        }
        
        StringBuilder sb = new StringBuilder(); 
        // For the case that k is larger than 0
        // Example: "112", k = 1
        for(int i = 0; i <= index-k; i++) sb.append(stack[i]);
        
        return sb.length() == 0? "0" : sb.toString();
    }
}
```