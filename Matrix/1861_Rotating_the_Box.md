# 1861. Rotating the Box
You are given an  `m x n`  matrix of characters  `box`  representing a side-view of a box. Each cell of the box is one of the following:

-   A stone  `'#'`
-   A stationary obstacle  `'*'`
-   Empty  `'.'`

The box is rotated  **90 degrees clockwise**, causing some of the stones to fall due to gravity. Each stone falls down until it lands on an obstacle, another stone, or the bottom of the box. Gravity  **does not**  affect the obstacles' positions, and the inertia from the box's rotation  **does not** affect the stones' horizontal positions.

It is  **guaranteed**  that each stone in  `box`  rests on an obstacle, another stone, or the bottom of the box.

Return  _an_ `n x m` _matrix representing the box after the rotation described above_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcodewithstones.png)

**Input:** box = ```[["#",".","#"]]```
**Output:** 
```
[["."],
["#"],
["#"]]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcode2withstones.png)

**Input:** box = 
```
[["#",".","*","."],
["#","#","*","."]]
              ```
**Output:** 
```
[["#","."],
["#","#"],
["*","*"],
[".","."]]
         ```

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcode3withstone.png)

**Input:** box = 
```
[["#","#","*",".","*","."],
["#","#","#","*",".","."],
["#","#","#",".","#","."]]
```
**Output:** 
```
[[".","#","#"],
[".","#","#"],
["#","#","*"],
["#","*","."],
["#",".","*"],
["#",".","."]]
```

**Constraints:**

-   `m == box.length`
-   `n == box[i].length`
-   `1 <= m, n <= 500`
-   `box[i][j]`  is either  `'#'`,  `'*'`, or  `'.'`.

# Solution:
```
class Solution {
    public char[][] rotateTheBox(char[][] box) {
        int m = box.length, n = box[0].length;
        char[][] res = new char[n][m];
        
        for(int i = 0; i < m; i++){
            int available = 0;
            for(int j = n - 1; j >= 0; j--){
                if(box[i][j] == '.') {
                    available = Math.max(j, available);
                    res[j][m-1-i] = '.';
                }else if(box[i][j] == '*') {
                    available = 0;
                    res[j][m-1-i] = '*';
                }
                else{
                    if(j < available) {
                        res[available][m-1-i] = '#';
                        res[j][m-1-i] = '.';
                    }else res[j][m-1-i] = '#';
                    available--;
                }
            }
        }
        
        return res;
    }
}
```