# 241. Different Ways to Add Parentheses
Given a string  `expression`  of numbers and operators, return  _all possible results from computing all the different possible ways to group numbers and operators_. You may return the answer in  **any order**.

**Example 1:**

**Input:** expression = "2-1-1"
**Output:** [0,2]
**Explanation:**
((2-1)-1) = 0 
(2-(1-1)) = 2

**Example 2:**

**Input:** expression = "2*3-4*5"
**Output:** [-34,-14,-10,-10,10]
**Explanation:**
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10

**Constraints:**

-   `1 <= expression.length <= 20`
-   `expression`  consists of digits and the operator  `'+'`,  `'-'`, and  `'*'`.
-   All the integer values in the input expression are in the range  `[0, 99]`.


# Solution 1: Recursion
```
public class Solution {
    public List<Integer> diffWaysToCompute(String input) {
        List<Integer> ret = new LinkedList<Integer>();
        for (int i=0; i<input.length(); i++) {
            if (input.charAt(i) == '-' ||
                input.charAt(i) == '*' ||
                input.charAt(i) == '+' ) {
                String part1 = input.substring(0, i);
                String part2 = input.substring(i+1);
                List<Integer> part1Ret = diffWaysToCompute(part1);
                List<Integer> part2Ret = diffWaysToCompute(part2);
                for (Integer p1 :   part1Ret) {
                    for (Integer p2 :   part2Ret) {
                        int c = 0;
                        switch (input.charAt(i)) {
                            case '+': c = p1+p2;
                                break;
                            case '-': c = p1-p2;
                                break;
                            case '*': c = p1*p2;
                                break;
                        }
                        ret.add(c);
                    }
                }
            }
        }
        if (ret.size() == 0) {
            ret.add(Integer.valueOf(input));
        }
        return ret;
    }
}
```

# Solution 2: DP
```
class Solution {
    public List<Integer> diffWaysToCompute(String input) {
        List<Integer> result=new ArrayList<>();
        if(input==null||input.length()==0)  return result;
        List<String> ops=new ArrayList<>();
        for(int i=0; i<input.length(); i++){
            int j=i;
            while(j<input.length()&&Character.isDigit(input.charAt(j)))
                j++;
            ops.add(input.substring(i, j));
            if(j!=input.length())   ops.add(input.substring(j, j+1));
            i=j;
        }
        int N=(ops.size()+1)/2; //num of integers
        ArrayList<Integer>[][] dp=(ArrayList<Integer>[][]) new ArrayList[N][N];
        for(int d=0; d<N; d++){
            if(d==0){
                for(int i=0; i<N; i++){
                    dp[i][i]=new ArrayList<>();
                    dp[i][i].add(Integer.valueOf(ops.get(i*2)));
                }
                continue;
            }
            for(int i=0; i<N-d; i++){
                dp[i][i+d]=new ArrayList<>();
                for(int j=i; j<i+d; j++){
                    ArrayList<Integer> left=dp[i][j], right=dp[j+1][i+d];
                    String operator=ops.get(j*2+1);
                    for(int leftNum:left)
                        for(int rightNum:right){
                            if(operator.equals("+"))
                                dp[i][i+d].add(leftNum+rightNum);
                            else if(operator.equals("-"))
                                dp[i][i+d].add(leftNum-rightNum);
                            else
                                dp[i][i+d].add(leftNum*rightNum);
                        }
                }
            }
        }
        return dp[0][N-1];
    }
}
```