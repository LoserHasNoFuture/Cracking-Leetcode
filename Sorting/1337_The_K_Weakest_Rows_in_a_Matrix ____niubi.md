# 1337. The K Weakest Rows in a Matrix
Given a  `m * n` matrix  `mat`  of  _ones_ (representing soldiers) and  _zeros_ (representing civilians), return the indexes of the  `k`  weakest rows in the matrix ordered from the weakest to the strongest.

A row  _**i**_  is weaker than row  _**j**_, if the number of soldiers in row  _**i**_  is less than the number of soldiers in row  _**j**_, or they have the same number of soldiers but  _**i**_  is less than  _**j**_. Soldiers are  **always**  stand in the frontier of a row, that is, always  _ones_ may appear first and then  _zeros_.

**Example 1:**

**Input:** mat = 
[[1,1,0,0,0],
 [1,1,1,1,0],
 [1,0,0,0,0],
 [1,1,0,0,0],
 [1,1,1,1,1]], 
k = 3
**Output:** [2,0,3]
**Explanation:** 
The number of soldiers for each row is: 
row 0 -> 2 
row 1 -> 4 
row 2 -> 1 
row 3 -> 2 
row 4 -> 5 
Rows ordered from the weakest to the strongest are [2,0,3,1,4]

**Example 2:**

**Input:** mat = 
[[1,0,0,0],
 [1,1,1,1],
 [1,0,0,0],
 [1,0,0,0]], 
k = 2
**Output:** [0,2]
**Explanation:** 
The number of soldiers for each row is: 
row 0 -> 1 
row 1 -> 4 
row 2 -> 1 
row 3 -> 1 
Rows ordered from the weakest to the strongest are [0,2,3,1]

**Constraints:**

-   `m == mat.length`
-   `n == mat[i].length`
-   `2 <= n, m <= 100`
-   `1 <= k <= m`
-   `matrix[i][j]`  is either 0  **or**  1.


# Solution 1: Priority Queue
Refer from: [https://leetcode.com/problems/the-k-weakest-rows-in-a-matrix/discuss/560163/Simple-Solution-In-Java](https://leetcode.com/problems/the-k-weakest-rows-in-a-matrix/discuss/560163/Simple-Solution-In-Java)
```
class Solution {
    public int[] kWeakestRows(int[][] mat, int k) {
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] != b[0] ? b[0] - a[0] : b[1] - a[1]);
        int[] ans = new int[k];
        
        for (int i = 0; i < mat.length; i++) {
            pq.offer(new int[] {numOnes(mat[i]), i});
            if (pq.size() > k)
                pq.poll();
        }
        
        while (k > 0)
            ans[--k] = pq.poll()[1];
        
        return ans;
    }
    
    private int numOnes(int[] row) {
        int lo = 0;
        int hi = row.length;
        
        while (lo < hi) {
            int mid = lo + (hi - lo) / 2;
            
            if (row[mid] == 1)
                lo = mid + 1;
            else
                hi = mid;
        }
        
        return lo;
    }
}
```

# Solution 2: Traverse Matrix Vertically
```
class Solution {
    public int[] kWeakestRows(int[][] mat, int k) {
        int rows = mat.length;
        int cols = mat[0].length;
        HashSet<Integer> set = new HashSet<Integer>();
        int[] res = new int[k];
        int index = 0;
        
        for(int j = 0; j < cols; j++){
            for(int i = 0; i < rows; i++){
                if(index == k) return res;
                if(mat[i][j] == 0 && !set.contains(i)){
                    res[index] = i;
                    set.add(i);
                    index++;
                }
            }
        }
        while(index < k){
            for(int i = 0; i < rows; i++){
                if(index == k) return res;
                if(mat[i][cols-1] == 1){
                    res[index] = i;
                    index++;
                }
            }
        }   
        return res;
    }
}
```

# Solution 3: Calculating Score (COOL!!)
Refer from: [https://leetcode.com/problems/the-k-weakest-rows-in-a-matrix/discuss/560163/Simple-Solution-In-Java](https://leetcode.com/problems/the-k-weakest-rows-in-a-matrix/discuss/560163/Simple-Solution-In-Java)
```
class Solution {
    public int[] kWeakestRows(int[][] mat, int k) {
        int rows = mat.length;
        int cols = mat[0].length;

        int[] score = new int[rows];
        int j;
        for (int i = 0; i < rows; i++) {
            j = numOnes(mat[i]);
            /*
             * we can create a score to match the sort condition from description
             * score = soldiersCount * rows + currentRowIndex
             * so we can get soldiersCount by score / rows, and get rowIndex by score % rows
             */
            score[i] = j * rows + i;
        }

        Arrays.sort(score);
        for (int i = 0; i < score.length; i++) {
            // get rowIndex
            score[i] = score[i] % rows;
        }

        return Arrays.copyOfRange(score, 0, k);
    }
    
    private int numOnes(int[] row) {
        int lo = 0;
        int hi = row.length;
        
        while (lo < hi) {
            int mid = lo + (hi - lo) / 2;
            
            if (row[mid] == 1)
                lo = mid + 1;
            else
                hi = mid;
        }
        
        return lo;
    }
}
```