# 1871. Jump Game VII
You are given a  **0-indexed**  binary string  `s`  and two integers  `minJump`  and  `maxJump`. In the beginning, you are standing at index  `0`, which is equal to  `'0'`. You can move from index  `i`  to index  `j`  if the following conditions are fulfilled:

-   `i + minJump <= j <= min(i + maxJump, s.length - 1)`, and
-   `s[j] == '0'`.

Return  `true` _if you can reach index_ `s.length - 1` _in_ `s`_, or_ `false` _otherwise._

**Example 1:**

**Input:** s = "011010", minJump = 2, maxJump = 3
**Output:** true
**Explanation:**
In the first step, move from index 0 to index 3. 
In the second step, move from index 3 to index 5.

**Example 2:**

**Input:** s = "01101110", minJump = 2, maxJump = 3
**Output:** false

**Constraints:**

-   `2 <= s.length <= 105`
-   `s[i]`  is either  `'0'`  or  `'1'`.
-   `s[0] == '0'`
-   `1 <= minJump <= maxJump < s.length`

# Solution 1: DP
### Bottom-up DP 1:
Ref from: [https://leetcode.com/problems/jump-game-vii/discuss/1224804/JavaC%2B%2BPython-One-Pass-DP](https://leetcode.com/problems/jump-game-vii/discuss/1224804/JavaC%2B%2BPython-One-Pass-DP)
```
class Solution {
    public boolean canReach(String s, int minJ, int maxJ) {
        int n = s.length(), pre = 0;
        int cnt = 0;
        boolean[] dp = new boolean[n];
        dp[0] = true;
        for (int i = 1; i < n; ++i) {
            if(cnt >= maxJ) return false;
            if (i >= minJ && dp[i - minJ])
                pre++;
            if (i > maxJ && dp[i - maxJ - 1])
                pre--;
            dp[i] = pre > 0 && s.charAt(i) == '0';
            if(dp[i]) cnt = 0;
            else cnt++;
        }
        return dp[n - 1];
    }
}
```

### Bottom-up DP 2:
Ref from: [https://leetcode.com/problems/jump-game-vii/discuss/1224681/Python3-Thinking-process-no-DP-needed](https://leetcode.com/problems/jump-game-vii/discuss/1224681/Python3-Thinking-process-no-DP-needed)

```
class Solution {
    public boolean canReach(String s, int minJ, int maxJ) {
        int n = s.length(), max = 0, cur = 0, cnt = 0;
        boolean[] dp = new boolean[n];
        dp[0] = true;
        
        while(cur < n){
            if(cnt >= maxJ) return false;
            if(dp[cur] && s.charAt(cur) == '0'){
                int next = Math.max(cur+minJ, max);
                while(next <= cur+maxJ && next < n){
                    if(s.charAt(next) == '0'){
                        dp[next] = true;
                        if(next == n-1) return true;
                    }
                    next++;
                }
                max = cur + maxJ;
                cnt = 0;
            }else{
                cnt++;
            }
            cur++;
        }
        
        return dp[n - 1];
    }
}
```
Can also be written using Queue
```
class Solution {
    public boolean canReach(String s, int minJ, int maxJ) {
        int n = s.length(), max = 0;
        Queue<Integer> queue = new LinkedList<Integer>();
        queue.offer(0);
        
        while(!queue.isEmpty()){
            int cur = queue.poll();
            int next = Math.max(max, cur + minJ);
            while(next < n && next <= cur + maxJ){
                if(s.charAt(next) == '0'){
                    if(next == n - 1) return true;
                    queue.offer(next);
                }
                next++;
            }
            max = cur + maxJ;
        }
        
        return false;
    }
}
```

### Top-Down DP:
```
class Solution {
    public boolean canReach(String s, int minJump, int maxJump) {
        int n = s.length(), cnt = 0;
        if(s.charAt(n-1) == '1') return false;
        boolean[] dp = new boolean[n];
        dp[n-1] = true;
        
        for(int i = n-2; i >= 0; i--){
            if(cnt >= maxJump) return false;
            if(s.charAt(i) == '1') {
                cnt++;
                continue;
            }
            for(int j = minJump; j <= maxJump && i + j < n; j++){
                if(dp[i+j]){
                    dp[i] = true;
                    cnt = 0;
                    break;
                }
            }
        }
        
        return dp[0];
    }
}
```