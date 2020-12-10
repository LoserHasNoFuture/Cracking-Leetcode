# 60. Permutation Sequence
The set  `[1, 2, 3, ..., n]`  contains a total of  `n!`  unique permutations.

By listing and labeling all of the permutations in order, we get the following sequence for  `n = 3`:

1.  `"123"`
2.  `"132"`
3.  `"213"`
4.  `"231"`
5.  `"312"`
6.  `"321"`

Given  `n`  and  `k`, return the  `kth`  permutation sequence.

**Example 1:**

**Input:** n = 3, k = 3
**Output:** "213"

**Example 2:**

**Input:** n = 4, k = 9
**Output:** "2314"

**Example 3:**

**Input:** n = 3, k = 1
**Output:** "123"

**Constraints:**

-   `1 <= n <= 9`
-   `1 <= k <= n!`

# Solution 1: BackTracking (Beat 12%)
```
class Solution {
    
    int steps;
    public String getPermutation(int n, int k) {
        StringBuilder sb = new StringBuilder();
        boolean[] visited = new boolean[n+1];
        
        steps = k;
        dfs(n,0,visited,sb,0);
        
        return sb.reverse().toString();
    }
    
    public boolean dfs(int n, int index, boolean[] visited,StringBuilder sb, int depth){
        if(depth == n && --steps == 0)  return true;
        
        for(int i = 1; i <= n; i++){
            if(!visited[i]){
                visited[i] = true;
                if(dfs(n,i,visited,sb,depth+1)){
                    sb.append(i);
                    return true;
                }
                visited[i] = false;
            }
        }
        
        return false;
    }
}
```

# Solution 2: Math + BackTracking (Beat 100%)
Using mathematic method to estimate k-th result
```
class Solution {
    int steps;
    public String getPermutation(int n, int k) {
        StringBuilder sb = new StringBuilder();
        boolean[] visited = new boolean[n+1];
        
        steps = k;
        dfs(n,0,visited,sb,0);
        
        return sb.reverse().toString();
    }
    
    public boolean dfs(int n, int index, boolean[] visited,StringBuilder sb, int depth){
        if(depth == n && --steps == 0)  return true;
        
        for(int i = 1; i <= n; i++){
            if(!visited[i]){
                int fac = factorial(n-depth-1);
                if(fac < steps){
                    steps -= fac;
                    continue;
                }
                visited[i] = true;
                if(dfs(n,i,visited,sb,depth+1)){
                    sb.append(i);
                    return true;
                }
                visited[i] = false;
            }
        }
        
        return false;
    }
    
    public int factorial(int n){
        int res = 1;
        for(int i = 2; i <= n; i++) res *= i;
        return res;
    }
}
```