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

# Solution 3: Rabin Karp + Binary Search (Alleged beat 100%)

Ref: [https://blog.csdn.net/lucylove3943/article/details/83491416?spm=1001.2014.3001.5501](https://blog.csdn.net/lucylove3943/article/details/83491416?spm=1001.2014.3001.5501)
Ref: [https://leetcode.com/problems/maximum-length-of-repeated-subarray/discuss/156891/Binary-Search-%2B-Rabin-Karp-%2B-Hash-Table-O(N-log-N)-Beats-100](https://leetcode.com/problems/maximum-length-of-repeated-subarray/discuss/156891/Binary-Search-%2B-Rabin-Karp-%2B-Hash-Table-O(N-log-N)-Beats-100)


# Solution 4: Another way of thinking (Beat 30%)
Ref: [https://leetcode.com/problems/maximum-length-of-repeated-subarray/discuss/109059/O(mn)-time-O(1)-space-solution](https://leetcode.com/problems/maximum-length-of-repeated-subarray/discuss/109059/O(mn)-time-O(1)-space-solution)
```
class Solution {
            // The initial position and the directions in which we slide. One step means shifting the top array by one position (index) to the right, or the the bottom array by one position (index) to the left:
        //          [1,2,3,2,1]   --> 
        //           <--    [3,2,1,4,7]  
        
        //          [1,2,3,2,1]    
        //                [3,2,1,4,7]  
        
        //          [1,2,3,2,1]      -->
        //     <--      [3,2,1,4,7]  

       //  and so on

    public int findLength(int[] A, int[] B) {
        int result = 0;
        for (int i = 0; i < A.length + B.length - 1; ++i) {
            // The current overlapping window is A[aStart, Math.min(A.length, B.length)] and B[bStart, Math.min(A.length, B.length)].
            int aStart = Math.max(0, A.length - 1 - i);  
            int bStart = Math.max(0, i - (A.length - 1));
            int numConsecutiveMatches = 0;
            for (int aIdx = aStart, bIdx = bStart; aIdx < A.length && bIdx < B.length; ++aIdx, ++bIdx) {
                // Maintain number of equal consecutive elements in the current window (overlap) and the max number ever computed.
                numConsecutiveMatches = A[aIdx] == B[bIdx] ? numConsecutiveMatches + 1 : 0;
                result = Math.max(result, numConsecutiveMatches);
            }
        }
        return result;
    }
}
```