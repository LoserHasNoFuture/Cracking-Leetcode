# 870. Advantage Shuffle
Given two arrays  `A`  and  `B`  of equal size, the  _advantage of  `A`  with respect to  `B`_  is the number of indices  `i` for which  `A[i] > B[i]`.

Return  **any**  permutation of  `A`  that maximizes its advantage with respect to  `B`.

**Example 1:**

**Input:** A = [2,7,11,15], B = [1,10,4,11]
**Output:** [2,11,7,15]

**Example 2:**

**Input:** A = [12,24,8,32], B = [13,25,32,11]
**Output:** [24,32,8,12]

**Note:**

1.  `1 <= A.length = B.length <= 10000`
2.  `0 <= A[i] <= 10^9`
3.  `0 <= B[i] <= 10^9`

# Solution: Tian Ji’s Strategy for a Horse Racing
**Better solution is to change the sorting algo to Radix Sort.**

田忌赛马
```
class Solution {
    public int[] advantageCount(int[] A, int[] B) {
        int n = A.length;
        int[] res = new int[n];
        
        PriorityQueue<int[]> pq = new PriorityQueue<>((a,b)->b[0]-a[0]);
        for (int i=0; i<n; i++) pq.add(new int[]{B[i], i});
        Arrays.sort(A);
        
        int start = 0, end = n-1;
        while(!pq.isEmpty()){
            int[] cur= pq.poll();
            if(A[end] > cur[0]){
                res[cur[1]] = A[end];
                end--;
            }else{
                res[cur[1]] = A[start];
                start++;
            }
        }
        
        return res;
    }
}
```