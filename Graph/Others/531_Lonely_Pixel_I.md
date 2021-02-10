# 531. Lonely Pixel I
Given an  `m x n`  `picture`  consisting of black  `'B'`  and white  `'W'`  pixels, return  _the number of  **black**  lonely pixels_.

A black lonely pixel is a character  `'B'`  that located at a specific position where the same row and same column don't have  **any other**  black pixels.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/11/pixel1.jpg)

**Input:** picture = [["W","W","B"],["W","B","W"],["B","W","W"]]
**Output:** 3
**Explanation:** All the three 'B's are black lonely pixels.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/11/pixel2.jpg)

**Input:** picture = [["B","B","B"],["B","B","B"],["B","B","B"]]
**Output:** 0

**Constraints:**

-   `m == picture.length`
-   `n == picture[i].length`
-   `1 <= m, n <= 500`
-   `picture[i][j]`  is  `'W'`  or  `'B'`.



# Solution 1: DFS (Beat 14%)
```
class Solution {
    HashSet<Integer> col = new HashSet<Integer>();
    HashSet<Integer> row = new HashSet<Integer>();
    
    public int findLonelyPixel(char[][] picture) {
        
        int res = 0;
        for(int i = 0; i < picture.length; i++){
            if(row.contains(i)) continue;
            for(int j = 0; j < picture[0].length; j++){
                if(col.contains(j)) continue;
                
                if(picture[i][j] == 'B'){
                    if(dfs(picture, i, j)) res++;
                }
                
            }
        }
        
        return res;
    }
    
    public boolean dfs(char[][] picture, int x, int y){
        boolean flag = true;
        for(int i = x+1; i < picture.length; i++){
            if(picture[i][y] == 'B'){
                flag = false;
                if(!row.contains(i)) {
                    row.add(i);
                    dfs(picture,i,y);
                }
            }
        }
        
        for(int i = y+1; i < picture[0].length; i++){
            if(picture[x][i] == 'B'){
                flag = false;
                if(!col.contains(i)) {
                    col.add(i);
                    dfs(picture,x,i);
                }
            }
        }
        
        return flag;
    }
}
```
# Solution 2: Brute Force (Beat 14%)
```
class Solution {
    
    HashSet<Integer> col = new HashSet<Integer>();
    HashSet<Integer> row = new HashSet<Integer>();
    public int findLonelyPixel(char[][] picture) {
        int res = 0;
        
        for(int i = 0; i < picture.length; i++){
            if(row.contains(i)) continue;
            for(int j = 0; j < picture[0].length; j++){
                if(col.contains(j)) continue;
                if(picture[i][j] == 'B'){
                    row.add(i);col.add(j);
                    if(any_in_col_and_row(picture,i,j)) res++;
                }
            }
        }
        
        return res;
    }
    
    
    public boolean any_in_col_and_row(char[][] picture, int x, int y){
        boolean flag = true;
        
        for(int i = 0; i < picture.length; i++){
            if(i == x) continue;
            if(picture[i][y] == 'B'){
                flag = false;
                row.add(i);
            }
        }
        
        for(int j = 0; j < picture[0].length; j++){
            if(j == y) continue;
            if(picture[x][j] == 'B'){
                flag = false;
                col.add(j);
            }
        }
        
        return flag;
    }
}
```

# Solution 3: Traverse whole Graph (Bear 100%)
Refer from: [https://leetcode.com/problems/lonely-pixel-i/discuss/100018/Java-O(nm)-time-with-O(n%2Bm)-Space-and-O(1)-Space-Solutions](https://leetcode.com/problems/lonely-pixel-i/discuss/100018/Java-O(nm)-time-with-O(n%2Bm)-Space-and-O(1)-Space-Solutions)
```
public int findLonelyPixel(char[][] picture) {
    int n = picture.length, m = picture[0].length;
    
    int[] rowCount = new int[n], colCount = new int[m];
    for (int i=0;i<n;i++) 
        for (int j=0;j<m;j++) 
            if (picture[i][j] == 'B') { rowCount[i]++; colCount[j]++; }

    int count = 0;
    for (int i=0;i<n;i++) 
        for (int j=0;j<m;j++) 
            if (picture[i][j] == 'B' && rowCount[i] == 1 && colCount[j] == 1) count++;
                
    return count;
}
```