# 1041. Robot Bounded In Circle
On an infinite plane, a robot initially stands at  `(0, 0)`  and faces north. The robot can receive one of three instructions:

-   `"G"`: go straight 1 unit;
-   `"L"`: turn 90 degrees to the left;
-   `"R"`: turn 90 degrees to the right.

The robot performs the  `instructions`  given in order, and repeats them forever.

Return  `true`  if and only if there exists a circle in the plane such that the robot never leaves the circle.

**Example 1:**

**Input:** instructions = "GGLLGG"
**Output:** true
**Explanation:** The robot moves from (0,0) to (0,2), turns 180 degrees, and then returns to (0,0).
When repeating these instructions, the robot remains in the circle of radius 2 centered at the origin.

**Example 2:**

**Input:** instructions = "GG"
**Output:** false
**Explanation:** The robot moves north indefinitely.

**Example 3:**

**Input:** instructions = "GL"
**Output:** true
**Explanation:** The robot moves from (0, 0) -> (0, 1) -> (-1, 1) -> (-1, 0) -> (0, 0) -> ...

**Constraints:**

-   `1 <= instructions.length <= 100`
-   `instructions[i]`  is  `'G'`,  `'L'`  or,  `'R'`.

# Solution 1: Intuitive (Beat 46%)
Run string once and see when it ends, which direction  the robot is racing.
1. facing north, then check if the robot returns  to (0,0)
2. facing east/west, run the string **four** times, , then check if the robot returns  to (0,0)
3. facing south, run the string **two** times, , then check if the robot returns  to (0,0)
```
class Solution {
    public boolean isRobotBounded(String ins) {
        int[] count = new int[2];
        for(char c: ins.toCharArray()){
            if(c == 'L') count[0]++;
            else if(c == 'R') count[1]++;
        }
        
        int loop = Math.abs(count[0]-count[1])%4;
        if(loop == 1 || loop == 3) loop = 4;
        else if(loop == 0) loop = 1;
        
        int[] pos = new int[]{0,0};
        int[][] directions = new int[][]{{0,1},{-1,0},{0,-1},{1,0}};
        int dir = 0;
        for(int i = 0; i < loop; i++){
            for(char c: ins.toCharArray()){
                if(c == 'L') dir = (dir + 1)%4;
                else if(c == 'R') dir = dir == 0? 3:dir-1;
                else{
                    pos[0] += directions[dir][0];
                    pos[1] += directions[dir][1];
                }
            }
        }
        
        return pos[0]==0&&pos[1]==0;
    }
}
```

# Solution 2: Think Deeper From Solution 1 (Beat 100%)
Refer from: [https://leetcode.com/problems/robot-bounded-in-circle/discuss/290856/JavaC%2B%2BPython-Let-Chopper-Help-Explain](https://leetcode.com/problems/robot-bounded-in-circle/discuss/290856/JavaC%2B%2BPython-Let-Chopper-Help-Explain)
Run the string one time, 
1. if it faces north, the robot must return to (0,0)
2. otherwise, the robot will always be walking in a circle
```
class Solution {
    public boolean isRobotBounded(String ins) {
        int x = 0, y = 0, i = 0, d[][] = {{0, 1}, {1, 0}, {0, -1}, { -1, 0}};
        for (int j = 0; j < ins.length(); ++j)
            if (ins.charAt(j) == 'R')
                i = (i + 1) % 4;
            else if (ins.charAt(j) == 'L')
                i = (i + 3) % 4;
            else {
                x += d[i][0]; y += d[i][1];
            }
        return x == 0 && y == 0 || i > 0;
    }
}
```