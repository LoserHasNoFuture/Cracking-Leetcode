# 842. Split Array into Fibonacci Sequence 306 (Not Finished)
Given a string  `S` of digits, such as  `S = "123456579"`, we can split it into a  _Fibonacci-like sequence_ `[123, 456, 579].`

Formally, a Fibonacci-like sequence is a list `F`  of non-negative integers such that:

-   `0 <= F[i] <= 2^31 - 1`, (that is, each integer fits a 32-bit signed integer type);
-   `F.length >= 3`;
-   and `F[i] + F[i+1] = F[i+2]` for all  `0 <= i < F.length - 2`.

Also, note that when splitting the string into pieces, each piece must not have extra leading zeroes, except if the piece is the number 0 itself.

Return any Fibonacci-like sequence split from  `S`, or return  `[]`  if it cannot be done.

**Example 1:**

**Input:** "123456579"
**Output:** [123,456,579]

**Example 2:**

**Input:** "11235813"
**Output:** [1,1,2,3,5,8,13]

**Example 3:**

**Input:** "112358130"
**Output:** []
**Explanation:** The task is impossible.

**Example 4:**

**Input:** "0123"
**Output:** []
**Explanation:** Leading zeroes are not allowed, so "01", "2", "3" is not valid.

**Example 5:**

**Input:** "1101111"
**Output:** [110, 1, 111]
**Explanation:** The output [11, 0, 11, 11] would also be accepted.

**Note:**

1.  `1 <= S.length <= 200`
2.  `S`  contains only digits.


# Solution 1:  BackTracking (Beat 75%)
**num = Long.valueOf(s.substring(tmp,++index));** has lower overhead than **num = num*10 + Integer.parseInt(s.charAt(index++)+"");**.
```
class Solution {
    ArrayList<Integer> res = new ArrayList<Integer>();
    public List<Integer> splitIntoFibonacci(String S) {
        ArrayList<Integer> cur = new ArrayList<Integer>();
        dfs(S, -1, -1, 0, cur);
        return res;
    }
    
    public void dfs(String s, int num1, int num2, int index, ArrayList<Integer> cur){
        if(res.size() != 0) return;
        if(index >= s.length()) {
            res = new ArrayList<Integer>(cur);
            return;
        }
        long num = 0;
        int tmp = index ;
        while(index < s.length()){
            num = Long.valueOf(s.substring(tmp,++index));
            if(num > Integer.MAX_VALUE) return;
            
            if(num1 != -1 && num2 != -1) {
                if(num == num1 + num2){
                    cur.add((int)num);
                    dfs(s, num2, (int)num, index, cur);
                    cur.remove(cur.size()-1);    
                }else if(num > num1+num2) return;
            }
            
            if(num1 == -1){
                if(index < s.length()){
                    cur.add((int)num);
                    dfs(s, (int)num, -1, index, cur);
                    cur.remove(cur.size()-1);
                }
            } else if(num2 == -1 && index < s.length() ) {
                cur.add((int)num);
                dfs(s, num1, (int)num, index, cur);
                cur.remove(cur.size()-1);
            }
            
            if(num == 0) return;
        }
        
        return ;
    }
}
```