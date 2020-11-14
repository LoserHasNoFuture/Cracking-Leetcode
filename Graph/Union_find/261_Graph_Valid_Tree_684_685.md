# 261. Graph Valid Tree 684 685
Given  `n`  nodes labeled from  `0`  to  `n-1`  and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

**Example 1:**

**Input:** `n = 5`, and `edges = [[0,1], [0,2], [0,3], [1,4]]`
**Output:** true

**Example 2:**

**Input:** `n = 5,` and `edges = [[0,1], [1,2], [2,3], [1,3], [1,4]]`
**Output:** false

**Note**: you can assume that no duplicate edges will appear in  `edges`. Since all edges are undirected,  `[0,1]`  is the same as  `[1,0]`  and thus will not appear together in  `edges`.

# Solution: Union Find
```
class Solution {
    public boolean validTree(int n, int[][] edges) {
        if(edges.length != n-1) return false;
        
        int[] id = new int[n];
        int[] sz = new int[n];
        for(int i = 0; i < n; i++) id[i] = i;
        
        for(int[] edge: edges){
            int root1 = find_root(id,edge[0]);
            int root2 = find_root(id,edge[1]);
            if(root1 == root2) return false;
            
            if(sz[root1] < sz[root2]) id[root1] = root2;
            else{
                id[root2] = root1;
                sz[root2] = Math.max(sz[root1],sz[root2]+1);
            }         
        }
        
        return true;
    }
    
    public int find_root(int[] id, int index){
        while(id[index] != index){
            id[index] = id[id[index]];
            index = id[index];
        }
        
        return index;
    }
}
```