# 1340. Jump Game V
Given an array of integers  `arr`  and an integer  `d`. In one step you can jump from index  `i`  to index:

-   `i + x`  where: `i + x < arr.length`  and  `0 < x <= d`.
-   `i - x`  where: `i - x >= 0`  and  `0 < x <= d`.

In addition, you can only jump from index  `i`  to index  `j` if  `arr[i] > arr[j]`  and  `arr[i] > arr[k]`  for all indices  `k`  between  `i`  and  `j`  (More formally  `min(i, j) < k < max(i, j)`).

You can choose any index of the array and start jumping. Return  _the maximum number of indices_ you can visit.

Notice that you can not jump outside of the array at any time.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/23/meta-chart.jpeg)

**Input:** arr = [6,4,14,6,8,13,9,7,10,6,12], d = 2
**Output:** 4
**Explanation:** You can start at index 10. You can jump 10 --> 8 --> 6 --> 7 as shown.
Note that if you start at index 6 you can only jump to index 7. You cannot jump to index 5 because 13 > 9. You cannot jump to index 4 because index 5 is between index 4 and 6 and 13 > 9.
Similarly You cannot jump from index 3 to index 2 or index 1.

**Example 2:**

**Input:** arr = [3,3,3,3,3], d = 3
**Output:** 1
**Explanation:** You can start at any index. You always cannot jump to any index.

**Example 3:**

**Input:** arr = [7,6,5,4,3,2,1], d = 1
**Output:** 7
**Explanation:** Start at index 0. You can visit all the indicies. 

**Example 4:**

**Input:** arr = [7,1,7,1,7,1], d = 2
**Output:** 2

**Example 5:**

**Input:** arr = [66], d = 1
**Output:** 1

**Constraints:**

-   `1 <= arr.length <= 1000`
-   `1 <= arr[i] <= 10^5`
-   `1 <= d <= arr.length`

# Solution 1: Intuitive DP O(nlogn + nd)
```
class Solution {
    public int maxJumps(int[] arr, int d) {
        int n = arr.length, res = 1;
        int[] dp = new int[n];
        Arrays.fill(dp,1);
        int[][] nums = new int[n][2];
        
        for(int i = 0; i < n; i++){
            nums[i][0] = arr[i];
            nums[i][1] = i;
        }
        Arrays.sort(nums, (a,b)->(a[0]-b[0]));
        
        
        for(int i = 0; i < n; i++){
            int index = nums[i][1];
            for(int j = 1; j <= d && index + j < n && arr[index] > arr[index+j]; j++){
                dp[index] = Math.max(dp[index], dp[index+j] + 1);
            }
            for(int j = 1; j <= d && index - j >= 0 && arr[index] > arr[index-j]; j++){
                dp[index] = Math.max(dp[index], dp[index-j] + 1);
            }
            res = Math.max(res, dp[index]);
        }
        
        return res;
    }
}
```

# Solution 2: Top-Down DP O(nd)
```
class Solution {
    public int maxJumps(int[] arr, int d) {
        int n = arr.length;
        int dp[] = new int[n];
        int res = 1;
        for (int i = 0; i < n; ++i)
            res = Math.max(res, dfs(arr, n, d, i, dp));
        return res;
    }

    private int dfs(int[] arr, int n, int d, int i, int[] dp) {
        if (dp[i] != 0) return dp[i];
        int res = 1;
        for (int j = i + 1; j <= Math.min(i + d, n - 1) && arr[j] < arr[i]; ++j)
            res = Math.max(res, 1 + dfs(arr, n, d, j, dp));
        for (int j = i - 1; j >= Math.max(i - d, 0) && arr[j] < arr[i]; --j)
            res = Math.max(res, 1 + dfs(arr, n, d, j, dp));
        return dp[i] = res;
    }
}
```
