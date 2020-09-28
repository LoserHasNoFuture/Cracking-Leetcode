# 967. Numbers With Same Consecutive Differences
Return all  **non-negative**  integers of length  `n`  such that the absolute difference between every two consecutive digits is  `k`.

Note that  **every**  number in the answer  **must not**  have leading zeros  **except**  for the number  `0`  itself. For example,  `01`  has one leading zero and is invalid, but  `0`  is valid.

You may return the answer in  **any order**.

**Example 1:**

**Input:** n = 3, k = 7
**Output:** [181,292,707,818,929]
**Explanation:** Note that 070 is not a valid number, because it has leading zeroes.

**Example 2:**

**Input:** n = 2, k = 1
**Output:** [10,12,21,23,32,34,43,45,54,56,65,67,76,78,87,89,98]

**Example 3:**

**Input:** n = 2, k = 0
**Output:** [11,22,33,44,55,66,77,88,99]

**Example 4:**

**Input:** n = 2, k = 1
**Output:** [10,12,21,23,32,34,43,45,54,56,65,67,76,78,87,89,98]

**Example 5:**

**Input:** n = 2, k = 2
**Output:** [13,20,24,31,35,42,46,53,57,64,68,75,79,86,97]

**Constraints:**

-   `2 <= n <= 9`
-   `0 <= k <= 9`

# Solution 1: BFS
```
class Solution {
    public int[] numsSameConsecDiff(int n, int k) {
        HashSet<Integer> res = new HashSet<Integer>();
        for(int i = 1; i <= 9; i++){
            Queue<Integer> q = new LinkedList<Integer>();
            q.offer(i);
            int bit = 1;
            while(!q.isEmpty() && bit < n){
                int sz = q.size();
                for(int j = 0; j < sz; j++){
                    int cur = q.poll();
                    if(cur%10 - k >= 0) q.offer(cur*10 + cur%10 - k);
                    if(cur%10 + k <= 9) q.offer(cur*10 + cur%10 + k);
                }
                bit++;
            }
            while(!q.isEmpty()) res.add(q.poll());
        }
        
        int len = res.size();
        int[] val = new int[len];
        int index = 0;
        for(Integer key : res){
            val[index] = key;
            index++;
        }
        
        return val;
    }
}
```


# Solution 2: DFS Recursion
```
class Solution {
    HashSet<Integer> res = new HashSet<Integer>();
    public int[] numsSameConsecDiff(int n, int k) {
        for(int i = 1; i <= 9; i++){
           dfs(i,k,n,1);
        }
        
        int len = res.size();
        int[] val = new int[len];
        int index = 0;
        for(Integer key : res){
            val[index] = key;
            index++;
        }
        
        return val;
    }
    
    public void dfs(int num, int k, int n, int num_digit){
        if(n == num_digit) {
            res.add(num);
            return;
        }
        int last_digit = num%10;
        if(last_digit - k >= 0) dfs(num*10 + last_digit - k, k, n, num_digit+1);
        if(last_digit + k <= 9) dfs(num*10 + last_digit + k, k, n, num_digit+1);
        
    }
}
```

# Solution: DFS Iteration
```
class Solution {
    public int[] numsSameConsecDiff(int n, int k) {
        HashSet<Integer> res = new HashSet<Integer>();
        int max = (int)Math.pow(10,n-1);
        
        for(int i = 1; i <= 9; i++){
            Deque<Integer> stack = new ArrayDeque<Integer>();
            stack.push(i);
            while(!stack.isEmpty()){
                int num = stack.pop();
                if(num/max != 0) {
                    res.add(num);
                    continue;
                }
                int last_digit = num%10;
                if(last_digit - k >= 0) stack.push(num*10 + last_digit - k);
                if(last_digit + k <= 9) stack.push(num*10 + last_digit + k);             
            }
        }
        
        int len = res.size();
        int[] val = new int[len];
        int index = 0;
        for(Integer key : res){
            val[index] = key;
            index++;
        }
        
        return val;
    }

}
```

