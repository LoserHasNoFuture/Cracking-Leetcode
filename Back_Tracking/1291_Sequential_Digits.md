# 1291. Sequential Digits
An integer has  _sequential digits_  if and only if each digit in the number is one more than the previous digit.

Return a  **sorted**  list of all the integers in the range  `[low, high]` inclusive that have sequential digits.

**Example 1:**

**Input:** low = 100, high = 300
**Output:** [123,234]

**Example 2:**

**Input:** low = 1000, high = 13000
**Output:** [1234,2345,3456,4567,5678,6789,12345]

**Constraints:**

-   `10 <= low <= high <= 10^9`

# Solution 1: BackTracking (Beat 100%)
```
class Solution {
    ArrayList<Integer> res = new ArrayList<Integer>();
    public List<Integer> sequentialDigits(int low, int high) {
        int cur = 0;
        
        for(int i = 1; i <= 9; i++)
            dfs(low, high, cur, i);
        
        Collections.sort(res);
        return res;
    }
    
    public void dfs(int low, int high, int cur, int digit){
        cur = cur*10 + digit;
        if(cur >= low && cur <= high) res.add(cur);
        if(cur > high) return;
        
        if(digit < 9) dfs(low,high,cur,digit+1);
    }
}
```