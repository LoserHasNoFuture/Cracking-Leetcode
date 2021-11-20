# 174. Dungeon Game
The demons had captured the princess (**P**) and imprisoned her in the bottom-right corner of a dungeon. The dungeon consists of M x N rooms laid out in a 2D grid. Our valiant knight (**K**) was initially positioned in the top-left room and must fight his way through the dungeon to rescue the princess.

The knight has an initial health point represented by a positive integer. If at any point his health point drops to 0 or below, he dies immediately.

Some of the rooms are guarded by demons, so the knight loses health (_negative_  integers) upon entering these rooms; other rooms are either empty (_0's_) or contain magic orbs that increase the knight's health (_positive_  integers).

In order to reach the princess as quickly as possible, the knight decides to move only rightward or downward in each step.

**Write a function to determine the knight's minimum initial health so that he is able to rescue the princess.**

For example, given the dungeon below, the initial health of the knight must be at least  **7**  if he follows the optimal path  `RIGHT-> RIGHT -> DOWN -> DOWN`.
| |   | |
|--|--|--|
|-2 ( K )| -3 |3|
| -5 |-10| 1|
|10|30|-5 ( P )|

**Note:**

-   The knight's health has no upper bound.
-   Any room can contain threats or power-ups, even the first room the knight enters and the bottom-right room where the princess is imprisoned.


# Solution1: Binary Search + DP
Simple Two-Dimension DP:
```
class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        if(dungeon.length == 0) return 1;

        int start = 1;
        int end = Integer.MAX_VALUE;
        
        while(start < end){
            int mid = (start + end) >>> 1;
            if(Hp_is_enough(dungeon,mid)) end = mid;
            else start = mid + 1;
        }
        
        return start;
    }
    
//     is there exits a path that knight won't die in the middle
//  If the knight's HP drops to zero along a path, then this path is not valid.
    public boolean Hp_is_enough(int[][] nums, int HP){
        int row = nums.length;
        int col = nums[0].length;
        
        int[][] dp = new int[row][col];
        dp[0][0] = HP + nums[0][0];
        if(dp[0][0] <= 0) return false;
        
        for(int i = 1; i < row; i++){
            if(dp[i-1][0] > 0) dp[i][0] = dp[i-1][0] + nums[i][0];
        }
        
        for(int i = 1; i < col; i++){
            if(dp[0][i-1] > 0) dp[0][i] = dp[0][i-1] + nums[0][i];
        }
        
        for(int i = 1; i < row; i++){
            boolean exist_one_path = false;
            if(dp[i][0] > 0) exist_one_path = true;
            for(int j = 1; j < col; j++){
                if(dp[i-1][j] > 0 || dp[i][j-1] > 0){
                    dp[i][j] = Math.max(dp[i-1][j],dp[i][j-1]) + nums[i][j];
                    if(dp[i][j] > 0) exist_one_path = true;
                }
            }
            if (!exist_one_path) return false;
        }
        
        return dp[row-1][col-1] > 0;
    }
}
```

Condense it to one dimension:
```
class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        if(dungeon.length == 0) return 1;

        int start = 1;
        int end = Integer.MAX_VALUE;
        
        while(start < end){
            int mid = (start + end) >>> 1;
            if(Hp_is_enough(dungeon,mid)) end = mid;
            else start = mid + 1;
        }
        
        return start;
    }
    
//     is there exits a path that knight won't die in the middle
    public boolean Hp_is_enough(int[][] nums, int HP){
        int row = nums.length;
        int col = nums[0].length;
        
        int[] dp = new int[col];
        dp[0] = HP + nums[0][0];
        if(dp[0] <= 0) return false;
        
        for(int i = 1; i < col; i++){
            if(dp[i-1] > 0) dp[i] = dp[i-1] + nums[0][i];
        }
        
        for(int i = 1; i < row; i++){
            boolean exist_one_path = false;
            if(dp[0] > 0) dp[0] = dp[0] + nums[i][0];
            if(dp[0] > 0) exist_one_path = true;
            
            for(int j = 1; j < col; j++){
                if(dp[j-1] > 0 || dp[j] > 0) {
                    dp[j] = Math.max(dp[j-1],dp[j]) + nums[i][j];
                    if(dp[j] > 0) exist_one_path = true;
                }
            } 
            if (!exist_one_path) return false;
        }
        
        return dp[col-1] > 0;
    }
}
```

# Solution 2: Bottom-up DP
Starting from Princess cell, caculate the minimal amount of HP entering such cell.
Add 1 at the end of program.
```
class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        if(dungeon.length == 0) return 1;
        int row = dungeon.length;
        int col = dungeon[0].length;
        
        int[] dp = new int[col];
        
        dp[col-1] = dungeon[row-1][col-1] > 0? 0: -(dungeon[row-1][col-1]);
        for(int j = col-2;  j >= 0; j--) {
            dp[j] = (dungeon[row-1][j] - dp[j+1]) > 0?  0: -(dungeon[row-1][j] - dp[j+1]);
        }
        
        for(int i = row - 2; i>= 0; i--){
            dp[col-1] = (dungeon[i][col-1]-dp[col-1]) > 0? 0: -(dungeon[i][col-1]-dp[col-1]);
            for(int j = col-2; j>=0; j--){
                int temp1 = dungeon[i][j] - dp[j] > 0? 0 : -(dungeon[i][j] - dp[j]);
                int temp2 = dungeon[i][j] - dp[j+1]> 0? 0 : -(dungeon[i][j] - dp[j+1]);
                dp[j] = Math.min(temp1,temp2);
            }
        }
        
        return dp[0] + 1;
    }
}
```

Instead of saving positive numbers, we save negative numbers or 0 at each cell:
```
class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        if(dungeon.length == 0) return 1;
        int row = dungeon.length;
        int col = dungeon[0].length;
        
        int[] dp = new int[col];
        
        dp[col-1] = dungeon[row-1][col-1] > 0? 0: dungeon[row-1][col-1];
        for(int j = col-2;  j >= 0; j--) {
            dp[j] = (dungeon[row-1][j] + dp[j+1]) > 0?  0: dungeon[row-1][j] + dp[j+1];
        }
        
        for(int i = row - 2; i>= 0; i--){
            dp[col-1] = (dungeon[i][col-1]+dp[col-1]) > 0? 0: dungeon[i][col-1]+dp[col-1];
            for(int j = col-2; j>=0; j--){
                int temp1 = dungeon[i][j] + dp[j] > 0? 0 : dungeon[i][j] + dp[j];
                int temp2 = dungeon[i][j] + dp[j+1]> 0? 0 : dungeon[i][j] + dp[j+1];
                dp[j] = Math.max(temp1,temp2);
            }
        }
        
        return Math.abs(dp[0]) + 1;
    }
}
```

