# 48. Rotate Image
You are given an  `n x n`  2D  `matrix`  representing an image, rotate the image by  **90**  degrees (clockwise).

You have to rotate the image  [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly.  **DO NOT**  allocate another 2D matrix and do the rotation.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/28/mat1.jpg)

**Input:** matrix = [[1,2,3],[4,5,6],[7,8,9]]
**Output:** [[7,4,1],[8,5,2],[9,6,3]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/28/mat2.jpg)

**Input:** matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
**Output:** [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]

**Example 3:**

**Input:** matrix = [[1]]
**Output:** [[1]]

**Example 4:**

**Input:** matrix = [[1,2],[3,4]]
**Output:** [[3,1],[4,2]]

**Constraints:**

-   `matrix.length == n`
-   `matrix[i].length == n`
-   `1 <= n <= 20`
-   `-1000 <= matrix[i][j] <= 1000`

# Solution 1: Direct Rotate With Lines (Beat 100%) 
 The corner position is very tricky
```
class Solution {
    public void rotate(int[][] mat) {
        int n = mat.length;
        for(int i = 0; i <= n/2; i++){
            int l = i, r = n-i-1;
            if(l >= r) continue;
            int[] next = new int[r-i+1];
            for(int j = l; j <= r; j++) next[j-l] = mat[l][j];
            next = move_to(mat,next,new int[]{l,r},new int[]{1,0});
            next = move_to(mat,next,new int[]{r,r},new int[]{0,-1});
            next = move_to(mat,next,new int[]{r,l},new int[]{-1,0});
            next = move_to(mat,next,new int[]{l,l},new int[]{0,1});
        }
    }
    
    public int[] move_to(int[][] mat, int[] val, int[] ori, int[] dir){
        int[] next = new int[val.length];
        for(int i = 0; i < val.length; i++){
            int x = ori[0]+dir[0]*i;
            int y = ori[1]+dir[1]*i;
            next[i] = mat[x][y];
            mat[x][y] = val[i];
        }
        // The corner position is very tricky
        next[0] = val[val.length-1];
        return next;
    }
}
```

# Solution 2: Direct Rotate (Beat 100%) 
```
class Solution {
    public void rotate(int[][] mat) {
        int n = mat.length;
        for(int i = 0; i <= n/2; i++){
            int l = i, r = n-i-1;
            for(int j = l; j < r; j++){
                int next = mat[l][j];
                next = move_to(mat,j,r,next);
                next = move_to(mat,r,r-(j-l),next);
                next = move_to(mat,r-(j-l),l,next);
                next = move_to(mat,l,j,next);
            }
        }
    }
    
    public int move_to(int[][] mat, int x, int y, int val){
        int tmp = mat[x][y];
        mat[x][y] = val;
        return tmp;
    }
}
```

# Solution 3: A Smarter Solution (Beat 100%)
Ref From: [https://leetcode.com/problems/rotate-image/discuss/18872/A-common-method-to-rotate-the-image](https://leetcode.com/problems/rotate-image/discuss/18872/A-common-method-to-rotate-the-image)
```
/*
 * clockwise rotate
 * first reverse up to down, then swap the symmetry 
 * 1 2 3     7 8 9     7 4 1
 * 4 5 6  => 4 5 6  => 8 5 2
 * 7 8 9     1 2 3     9 6 3
*/
void rotate(vector<vector<int> > &matrix) {
    reverse(matrix.begin(), matrix.end());
    for (int i = 0; i < matrix.size(); ++i) {
        for (int j = i + 1; j < matrix[i].size(); ++j)
            swap(matrix[i][j], matrix[j][i]);
    }
}

/*
 * anticlockwise rotate
 * first reverse left to right, then swap the symmetry
 * 1 2 3     3 2 1     3 6 9
 * 4 5 6  => 6 5 4  => 2 5 8
 * 7 8 9     9 8 7     1 4 7
*/
void anti_rotate(vector<vector<int> > &matrix) {
    for (auto vi : matrix) reverse(vi.begin(), vi.end());
    for (int i = 0; i < matrix.size(); ++i) {
        for (int j = i + 1; j < matrix[i].size(); ++j)
            swap(matrix[i][j], matrix[j][i]);
    }
}
```