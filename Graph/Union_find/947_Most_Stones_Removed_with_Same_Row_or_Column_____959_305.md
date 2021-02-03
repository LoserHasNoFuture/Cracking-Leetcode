# 947. Most Stones Removed with Same Row or Column
On a 2D plane, we place  `n`  stones at some integer coordinate points. Each coordinate point may have at most one stone.

A stone can be removed if it shares either  **the same row or the same column**  as another stone that has not been removed.

Given an array  `stones`  of length  `n`  where  `stones[i] = [xi, yi]`  represents the location of the  `ith`  stone, return  _the largest possible number of stones that can be removed_.

**Example 1:**

**Input:** stones = [[0,0],[0,1],[1,0],[1,2],[2,1],[2,2]]
**Output:** 5
**Explanation:** One way to remove 5 stones is as follows:
1. Remove stone [2,2] because it shares the same row as [2,1].
2. Remove stone [2,1] because it shares the same column as [0,1].
3. Remove stone [1,2] because it shares the same row as [1,0].
4. Remove stone [1,0] because it shares the same column as [0,0].
5. Remove stone [0,1] because it shares the same row as [0,0].
Stone [0,0] cannot be removed since it does not share a row/column with another stone still on the plane.

**Example 2:**

**Input:** stones = [[0,0],[0,2],[1,1],[2,0],[2,2]]
**Output:** 3
**Explanation:** One way to make 3 moves is as follows:
1. Remove stone [2,2] because it shares the same row as [2,0].
2. Remove stone [2,0] because it shares the same column as [0,0].
3. Remove stone [0,2] because it shares the same row as [0,0].
Stones [0,0] and [1,1] cannot be removed since they do not share a row/column with another stone still on the plane.

**Example 3:**

**Input:** stones = [[0,0]]
**Output:** 0
**Explanation:** [0,0] is the only stone on the plane, so you cannot remove it.

**Constraints:**

-   `1 <= stones.length <= 1000`
-   `0 <= xi, yi  <= 104`
-   No two stones are at the same coordinate point.


# Solution : Union Find
### using two hashmaps to represent column and row (Beat 81%)
```
class Solution {
    int[] id, sz;
    int count;
    public int removeStones(int[][] stones) {
        int n = stones.length;
        if(n <= 1) return 0;
        
        count = n;
        id = new int[n];
        for(int i = 0; i < n; i++) id[i] = i;
        sz = new int[n];
        
        HashMap<Integer,Integer> rows = new HashMap<Integer,Integer>();
        HashMap<Integer,Integer> cols = new HashMap<Integer,Integer>();
        
        for(int i = 0; i < n; i++){
            int rowid = rows.getOrDefault(stones[i][0],i);
            int colid = cols.getOrDefault(stones[i][1],i);
            if(i != rowid && i != colid) count--;
            
            if(rowid != colid) id[i] = union(rowid,colid);
            else id[i]  = rowid;
            
            
            rows.put(stones[i][0],id[i]);
            cols.put(stones[i][1],id[i]);
        }
        
        return n - count;
    }
    
    public int find(int p){
        while(id[p] != p){
            id[p] = id[id[p]];
            p = id[p];
        }
        return p;
    }
    
    public int union(int p, int q){
        int yp = find(p);
        int yq = find(q);
        if(yq != yp){
            count--;
            if(sz[yp] > sz[yq]) id[yq] = yp;
            else{
                id[yp] = yq;
                sz[yq] = Math.max(sz[yq], sz[yp] + 1); 
                return yq;
            }
        }
        return yp;
        
    }
}
```

### using one hashmap (Beat 71%)
```
class Solution {
    HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
    int count = 0;
    public int removeStones(int[][] stones) {
        int n = stones.length;
        if(n <= 1) return 0;
       
        for(int i = 0; i < n; i++) union(stones[i][0],-stones[i][1]-1);
        
        return n - count;
    }
    
    public int find(int p){
        if(map.getOrDefault(p,Integer.MIN_VALUE) == Integer.MIN_VALUE){
            count++;
            map.put(p,p);
        }
        while(map.get(p) != p) {
            map.put(p, map.get(map.get(p)));
            p = map.get(p);
        }
        
        return p;
    }
    
    public void union(int p, int q){
        int yp = find(p);
        int yq = find(q);
        if(yq != yp){
            count--;
            map.put(yq,yp);
        }      
    }
    
    
}
```