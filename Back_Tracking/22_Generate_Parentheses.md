# 22. Generate Parentheses
Given  `n`  pairs of parentheses, write a function to  _generate all combinations of well-formed parentheses_.

**Example 1:**

**Input:** n = 3
**Output:** ["((()))","(()())","(())()","()(())","()()()"]

**Example 2:**

**Input:** n = 1
**Output:** ["()"]

**Constraints:**

-   `1 <= n <= 8`

# Solution 1: BackTracking (Beat 100%)
```
class Solution {
    ArrayList<String> res = new ArrayList<String>();
    public List<String> generateParenthesis(int n) {
        StringBuilder sb = new StringBuilder();
        int[] un_visited = new int[2];
        for(int i = 0; i < 2; i++) un_visited[i] = n;
        
        un_visited[0]--;
        dfs(un_visited,sb,0,2*n-1);
        return res;
    }
    
    public void dfs(int[] un_visited,StringBuilder sb, int index, int len){
        if(len < 0 || un_visited[0] > un_visited[1]) return;
        if(index == 0) sb.append('(');
        else sb.append(')');
        
        if(len == 0) res.add(sb.toString());
        
        for(int i = 0; i < 2; i++){
            if(un_visited[i] > 0 && len > 0){
                un_visited[i]--;
                dfs(un_visited,sb,i,len-1);
                un_visited[i]++;
            }
        }
             
        sb.delete(sb.length()-1,sb.length());
    }
}
```