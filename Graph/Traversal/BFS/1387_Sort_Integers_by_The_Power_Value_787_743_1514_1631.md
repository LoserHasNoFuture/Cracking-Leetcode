# 1387. Sort Integers by The Power Value
The power of an integer  `x`  is defined as the number of steps needed to transform `x`  into  `1`  using the following steps:

-   if  `x`  is even then  `x = x / 2`
-   if  `x`  is odd then  `x = 3 * x + 1`

For example, the power of x = 3 is 7 because 3 needs 7 steps to become 1 (3 --> 10 --> 5 --> 16 --> 8 --> 4 --> 2 --> 1).

Given three integers  `lo`,  `hi`  and  `k`. The task is to sort all integers in the interval  `[lo, hi]`  by the power value in  **ascending order**, if two or more integers have  **the same**  power value sort them by  **ascending order**.

Return the  `k-th`  integer in the range  `[lo, hi]`  sorted by the power value.

Notice that for any integer  `x`  `(lo <= x <= hi)`  it is  **guaranteed**  that  `x`  will transform into  `1`  using these steps and that the power of  `x`  is will  **fit**  in 32 bit signed integer.

**Example 1:**

**Input:** lo = 12, hi = 15, k = 2
**Output:** 13
**Explanation:** The power of 12 is 9 (12 --> 6 --> 3 --> 10 --> 5 --> 16 --> 8 --> 4 --> 2 --> 1)
The power of 13 is 9
The power of 14 is 17
The power of 15 is 17
The interval sorted by the power value [12,13,14,15]. For k = 2 answer is the second element which is 13.
Notice that 12 and 13 have the same power value and we sorted them in ascending order. Same for 14 and 15.

**Example 2:**

**Input:** lo = 1, hi = 1, k = 1
**Output:** 1

**Example 3:**

**Input:** lo = 7, hi = 11, k = 4
**Output:** 7
**Explanation:** The power array corresponding to the interval [7, 8, 9, 10, 11] is [16, 3, 19, 6, 14].
The interval sorted by power is [8, 10, 11, 7, 9].
The fourth number in the sorted array is 7.

**Example 4:**

**Input:** lo = 10, hi = 20, k = 5
**Output:** 13

**Example 5:**

**Input:** lo = 1, hi = 1000, k = 777
**Output:** 570

**Constraints:**

-   `1 <= lo <= hi <= 1000`
-   `1 <= k <= hi - lo + 1`

# Solution 1: Recursion + HashMap
[https://leetcode.com/problems/sort-integers-by-the-power-value/discuss/613036/Java-PQ](https://leetcode.com/problems/sort-integers-by-the-power-value/discuss/613036/Java-PQ)
```
class Solution {
    Map<Integer,Integer> map; // Store steps for each value to reach 1. So that it can be reused.

    public int getKth(int lo, int hi, int k) {
        map = new HashMap<>();
        PriorityQueue<int[]> pq =
                new PriorityQueue<>((a,b)-> b[1]!=a[1] ?a[1]-b[1]:a[0]-b[0]);
        for (int i=lo;i<=hi;i++) {
            pq.add(new int[]{i,util(i)});
        }
        while (!pq.isEmpty() && k-->1) { 
            pq.poll();
        }
        return pq.poll()[0];
    }

    private int util(int val) {
        if(val==1) {
            return 0;
        }
        if(map.containsKey(val)) {
            return map.get(val);
        }
        if((val&1)==0) {
            map.put(val,util(val/2)+1);
        } else {
            map.put(val,util(3*val+1)+1);
        }
        return map.get(val);
    }
}
```

# Solution 2: Dijistra's Algorithm
First converting the problem to a weighted graph, and using dijistra's algorithm to solve it

As explained in the program:

```
class Solution {
    public int getKth(int lo, int hi, int k) {
        if(lo == hi) return lo;
        int n = hi - lo + 1;
		
		// Adding an extra edge[n] to save node 1, (as graph's starting point)
        ArrayList<int[]>[] edges = new ArrayList[n+1];
        for(int i = 0; i < n+1; i++) edges[i] = new ArrayList<int[]>();
		boolean[] visited = new boolean[n];
        
		// Adding links to the graph (actually it is also a binary tree)
        for(int i = lo; i <= hi; i++){
            if(visited[i-lo]) continue;
            
			// cnt is the distance of the egde
            int cnt = 0, temp = i;
            int prev = temp;
            while(temp != 1){
                cnt++;
                if((temp&1) == 1) temp = temp*3 + 1;
                else temp = temp >>> 1;
                
                if(temp >= lo && temp <= hi){
                    visited[prev - lo] = true;
                    edges[temp-lo].add(new int[]{prev,cnt});
                    if(visited[temp-lo]) break;
                    cnt = 0;
                    prev = temp;
                }
            }
            
            if(temp == 1 && !visited[prev-lo]) {
                visited[prev-lo] = true;
                edges[n].add(new int[]{prev, cnt});
            }
        }
        

//     BFS + PriorityQueue  = Dijistra's Algorithm
        PriorityQueue<int[]> queue = new PriorityQueue<int[]>((a,b)->(a[1]==b[1]? a[0]-b[0]: a[1]-b[1]));
        for(int[] edge: edges[n]) queue.offer(edge);
        
        while(!queue.isEmpty()){
            int[] cur = queue.poll();
            if(--k == 0) return cur[0];
            for(int[] edge: edges[cur[0]-lo]){
                edge[1] += cur[1];
                queue.offer(edge);    
            }
        }
      
        return -1;
    }
}
```