# 765. Couples Holding Hands
N couples sit in 2N seats arranged in a row and want to hold hands. We want to know the minimum number of swaps so that every couple is sitting side by side. A  _swap_  consists of choosing  **any**  two people, then they stand up and switch seats.

The people and seats are represented by an integer from  `0`  to  `2N-1`, the couples are numbered in order, the first couple being  `(0, 1)`, the second couple being  `(2, 3)`, and so on with the last couple being  `(2N-2, 2N-1)`.

The couples' initial seating is given by  `row[i]`  being the value of the person who is initially sitting in the i-th seat.

**Example 1:**  

**Input:** row = [0, 2, 1, 3]
**Output:** 1
**Explanation:** We only need to swap the second (row[1]) and third (row[2]) person.

**Example 2:**  

**Input:** row = [3, 2, 0, 1]
**Output:** 0
**Explanation:** All couples are already seated side by side.

**Note:**

1.  `len(row)`  is even and in the range of  `[4, 60]`.
2.  `row`  is guaranteed to be a permutation of  `0...len(row)-1`.

# Solution 1: 
```
class Solution {
    public int minSwapsCouples(int[] row) {
        int res = 0;
        for(int i = 0; i < row.length; i = i + 2){
            int couple = row[i] ^ 1;
            if(row[i+1] == couple) continue;
            int index = find_couple(row,couple,i+2);
            swap(row, i+1, index);
            res++;
        }
        return res;
    }
    
    public void swap(int[] row, int a, int b){
        int temp = row[a];
        row[a] = row[b];
        row[b] = temp;
    } 

    public int find_couple(int[] row, int target, int start){
        for(int i = start; i < row.length; i++){
            if(target == row[i]) return i;
        }
        return -1;
    }
}
```


# Solution: Union Find
```
class Solution {
    public int minSwapsCouples(int[] row) {
        if(row.length == 0) return 0;
        int[] id = new int[row.length/2];
        int[] sz = new int[row.length/2];
        HashSet<Integer> set = new HashSet<>();
        for(int i = 0; i < id.length; i++){
            id[i] = i;
            set.add(i);
        } 
        
        for(int i = 0; i < row.length; i = i+2){
            int x = find_root(id,row[i]/2);
            int y = find_root(id,row[i+1]/2);
            if(x == y) continue;
            if(sz[x] < sz[y]){
                id[x] = y;
                set.remove(x);
            } else{
                id[y] = x;
                set.remove(y);
                sz[x] = Math.max(sz[x],sz[y]+1);
            }
        }
        return row.length/2 - set.size();
    }
    
    public int find_root(int[] id, int p){
        while(id[p] != p){
            id[p] = id[id[p]];
            p = id[p];
        }
        return p;
    }
}
```
