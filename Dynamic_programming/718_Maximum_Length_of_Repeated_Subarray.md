# 718. Maximum Length of Repeated Subarray
Given two integer arrays  `A`  and  `B`, return the maximum length of an subarray that appears in both arrays.

**Example 1:**

**Input:**
A: [1,2,3,2,1]
B: [3,2,1,4,7]
**Output:** 3
**Explanation:** 
The repeated subarray with maximum length is [3, 2, 1].

**Note:**

1.  1 <= len(A), len(B) <= 1000
2.  0 <= A[i], B[i] < 100

# Solution: Others' DP
```
class Solution {
    public int findLength(int[] A, int[] B) {
        int m = A.length, n = B.length;
          //dp[i][j] is the length of longest common subarray ending with A[i-1] and B[j-1]
        int[][] dp = new int[m + 1][n + 1];
        int max = 0;
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (A[i - 1] == B[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                    max = Math.max(max, dp[i][j]);
                }
            }
        }
        return max;
    }
}
```
Condense to one dimension arr:
```
class Solution {
    public int findLength(int[] A, int[] B) {
        int m = A.length, n = B.length;
          //dp[i][j] is the length of longest common subarray ending with A[i-1] and B[j-1]
        int[] dp = new int[n + 1];
        int max = 0;
        for (int i = 1; i <= m; i++) {
            for (int j = n; j >= 1; j--) {
                if (A[i - 1] == B[j - 1]) {
                    dp[j] = dp[j - 1] + 1;
                    max = Math.max(max, dp[j]);
                }else dp[j] = 0;
            }
        }
        return max;
    }
}
```

We can further condense it to O(1) space.


# Solution: My lame DP
```
class Solution {
    public int findLength(int[] A, int[] B) {
        int res = 0;
        HashMap<Integer,ArrayList> map = new HashMap<Integer,ArrayList>();
        int lenA = A.length;
        int lenB = B.length;
        
        for(int i = 0; i < lenA; i++){
            ArrayList<Integer> arr = map.getOrDefault(A[i],new ArrayList<Integer>());
            arr.add(i);
            map.put(A[i],arr);
        }
        
        int[][] dp = new int[100][lenA];
        
        for(int i = 0; i < lenB; i++){
            ArrayList<Integer> arr = map.getOrDefault(B[i],null);
            if(arr == null) continue;
            for(int j = arr.size()-1; j >= 0; j--){
                int A_pos = arr.get(j);
                if(A_pos == 0 || i == 0) dp[B[i]][A_pos] = 1;
                else dp[B[i]][A_pos] = dp[B[i-1]][A_pos-1]+1;
                res = Math.max(res,dp[B[i]][A_pos]);
            }
        }
        
        return res;
    }
}
```