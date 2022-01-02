# 1510. Stone Game IV 1025
Alice and Bob take turns playing a game, with Alice starting first.

Initially, there are  `n`  stones in a pile. On each player's turn, that player makes a  _move_  consisting of removing  **any**  non-zero  **square number**  of stones in the pile.

Also, if a player cannot make a move, he/she loses the game.

Given a positive integer  `n`, return  `true`  if and only if Alice wins the game otherwise return  `false`, assuming both players play optimally.

**Example 1:**

**Input:** n = 1
**Output:** true
**Explanation:** Alice can remove 1 stone winning the game because Bob doesn't have any moves.

**Example 2:**

**Input:** n = 2
**Output:** false
**Explanation:** Alice can only remove 1 stone, after that Bob removes the last one winning the game (2 -> 1 -> 0).

**Example 3:**

**Input:** n = 4
**Output:** true
**Explanation:** n is already a perfect square, Alice can win with one move, removing 4 stones (4 -> 0).

**Constraints:**

-   `1 <= n <= 105`

# Solution: Dynamic Programming
```
class Solution {
    public boolean winnerSquareGame(int n) {
        if(n <= 3) return n%2 != 0;
        boolean[] dp = new boolean[n+1];
        dp[1] = true; dp[2] = false; dp[3] = true;
        
        for(int i = 4; i <= n; i++){
            for(int j = 1; j * j <= i; j++){
                if(!dp[i-j*j]){
                    dp[i] = true;
                    break;
                }
            }
        }
        
        return dp[n];
    }
}
```