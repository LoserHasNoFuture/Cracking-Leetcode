# 1025. Divisor Game 1510
Alice and Bob take turns playing a game, with Alice starting first.

Initially, there is a number  `n`  on the chalkboard. On each player's turn, that player makes a move consisting of:

-   Choosing any  `x`  with  `0 < x < n`  and  `n % x == 0`.
-   Replacing the number  `n`  on the chalkboard with  `n - x`.

Also, if a player cannot make a move, they lose the game.

Return  `true`  _if and only if Alice wins the game, assuming both players play optimally_.

**Example 1:**

**Input:** n = 2
**Output:** true
**Explanation:** Alice chooses 1, and Bob has no more moves.

**Example 2:**

**Input:** n = 3
**Output:** false
**Explanation:** Alice chooses 1, Bob chooses 1, and Alice has no more moves.

**Constraints:**

-   `1 <= n <= 1000`

# Solution: Dynamic Programming
```
class Solution {
    Boolean[] memo;
    public boolean divisorGame(int n) {
        if(n == 1) return false;
        if(n == 2) return true;
        memo = new Boolean[n+1];
        memo[1] = false; memo[2] = true;
        
        return dfs(n);
    }
    
    public boolean dfs(int n){
        if(memo[n] != null) return memo[n];
        
        memo[n] = false;
        for(int i = 1; i <= n/2; i++){
            if(n%i == 0 && !dfs(n-i)){
                memo[n] = true;
                break;
            }
        }
        
        return memo[n];
    }
}
```

# Solution 2: 
```
class Solution {
    public boolean divisorGame(int N) {
        
        return N%2 == 0;
    }
}
```