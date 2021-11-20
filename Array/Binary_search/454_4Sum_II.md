# 454. 4Sum II
Given four lists A, B, C, D of integer values, compute how many tuples  `(i, j, k, l)`  there are such that  `A[i] + B[j] + C[k] + D[l]`  is zero.

To make problem a bit easier, all A, B, C, D have same length of N where 0 ≤ N ≤ 500. All integers are in the range of -228  to 228  - 1 and the result is guaranteed to be at most 231  - 1.

**Example:**

**Input:**
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

**Output:**
2

**Explanation:**
The two tuples are:
1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0

# Solution 1: HashMap
```
class Solution {
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        int res = 0; 
        HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
        
        for(int a : A) {
            for(int b : B){
                map.put(a+b, map.getOrDefault(a+b,0)+1);
            }
        }
        
        for(int c : C) {
            for(int d : D){
                res += map.getOrDefault(-(c+d),0);
            }
        }
           
        return res;
    }
}
```

# Solution 2: Binary Search
```
class Solution {
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        int res = 0;
        
        int[] sumAB = new int[A.length*B.length];
        int i = 0;
        for(int a : A) {
            for(int b : B){
                sumAB[i++] = a+b;
            }
        }
        Arrays.sort(sumAB);
        
        int[] sumCD = new int[C.length*D.length];
        i = 0;
        for(int c : C) {
            for(int d : D){
                sumCD[i++] = c+d;
            }
        }
        Arrays.sort(sumCD);
        

        int prev = 0;
        for(i = 0; i < sumAB.length; i++){
            if(i > 0 && sumAB[i] == sumAB[i-1]) res+= prev;
            else{
                int start = first_in_sumCD(sumCD,-sumAB[i]);
                int end = last_in_sumCD(sumCD,-sumAB[i]);
                prev = (start == sumCD.length || end == -1) ? 0: (end-start + 1);
                res += prev;
            }
        }
        
        return res;
    }
    
    public int first_in_sumCD(int[] nums, int target){
        int start = 0;
        int end = nums.length;
        
        while(start < end){
            int mid = (start + end) >>> 1;
            if(nums[mid] >= target) end = mid;
            else start = mid + 1;
        }
        
        return start;
    }
    
    public int last_in_sumCD(int[] nums, int target){
        int start = -1;
        int end = nums.length - 1;
        
        while(start < end){
            int mid = (start + end + 1) >>> 1;
            if(nums[mid] <= target) start = mid;
            else end = mid - 1;
        }
        
        return start;
    }
}
```