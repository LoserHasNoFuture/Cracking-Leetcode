# 128. Longest Consecutive Sequence
Given an unsorted array of integers  `nums`, return  _the length of the longest consecutive elements sequence._

You must write an algorithm that runs in `O(n)` time.

**Example 1:**

**Input:** nums = [100,4,200,1,3,2]
**Output:** 4
**Explanation:** The longest consecutive elements sequence is `[1, 2, 3, 4]`. Therefore its length is 4.

**Example 2:**

**Input:** nums = [0,3,7,2,5,8,4,6,0,1]
**Output:** 9

**Constraints:**

-   `0 <= nums.length <= 105`
-   `-109  <= nums[i] <= 109`

# Solution 1: Union Find (Beat 5%)
A good way of thinking.
```
class Solution {
    public int longestConsecutive(int[] nums) {
        if(nums.length == 0) return 0;
        HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
        int[] id = new int[nums.length], sz = new int[nums.length];
        
        for(int i = 0; i < nums.length; i++) {
            map.put(nums[i],i);
            id[i] = i;
            sz[i] = 1;
        }
        
        int max = 1;
        for(int i = 0; i < nums.length; i++){
            max = Math.max(connect_to_neighbors(id,sz,map,nums[i]+1,nums[i]),max);
            max = Math.max(connect_to_neighbors(id,sz,map,nums[i]-1,nums[i]),max);
        }
        
        // System.out.println(Arrays.toString(sz));
        return max;
    }
    
    public int connect_to_neighbors(int[] id, int[] sz, 
                                    HashMap<Integer,Integer> map, int neigh, int cur){
        int x = find_root(id,map.get(cur));
            
        if(map.containsKey(neigh)){
            int y = find_root(id, map.get(neigh));
            if(x != y){
                if(sz[x] > sz[y]){
                    id[y] = x;
                    sz[x] += sz[y];
                    return sz[x];
                }else{
                    id[x] = y;
                    sz[y] += sz[x];
                    return sz[y];
                }
            }
        }
        return 0;
    }
    
    public int find_root(int[] id, int p){
        while(p != id[p]){
            id[p] = id[id[p]];
            p = id[p];
        }
        return p;
    }
}
```

# Solution 2: HashSet O(n) Solution (Beat 65%)
```
public class Solution {
    public int longestConsecutive(int[] nums) {
        if(nums == null || nums.length == 0) return 0;

        Set<Integer> set = new HashSet<Integer>();

        for(int num: nums) set.add(num);
        int max = 1;
        for(int num: nums) {
            if(set.contains(num) && !set.contains(num-1)) {//num hasn't been visited
                set.remove(num);
                int val = num+1;
                int sum = 1;
                
                while(set.contains(val)){
                    set.remove(val++);
                    sum++;
                }
                
                max = Math.max(sum,max);
            }
        }
        return max;
    }
}
```