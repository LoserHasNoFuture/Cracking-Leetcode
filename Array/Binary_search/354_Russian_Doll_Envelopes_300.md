# 354. Russian Doll Envelopes  300
You are given a 2D array of integers  `envelopes`  where  `envelopes[i] = [wi, hi]`  represents the width and the height of an envelope.

One envelope can fit into another if and only if both the width and height of one envelope are greater than the other envelope's width and height.

Return  _the maximum number of envelopes you can Russian doll (i.e., put one inside the other)_.

**Note:**  You cannot rotate an envelope.

**Example 1:**

**Input:** envelopes = [[5,4],[6,4],[6,7],[2,3]]
**Output:** 3
**Explanation:** The maximum number of envelopes you can Russian doll is `3` ([2,3] => [5,4] => [6,7]).

**Example 2:**

**Input:** envelopes = [[1,1],[1,1],[1,1]]
**Output:** 1

**Constraints:**

-   `1 <= envelopes.length <= 5000`
-   `envelopes[i].length == 2`
-   `1 <= wi, hi  <= 104`

# Solution 1: Sort + Problem 300 
DP: Beat 37%
```
class Solution {
    public int maxEnvelopes(int[][] env) {
        Arrays.sort(env,new Comparator<>(){
            public int compare(int[] a, int[] b){
                return a[0] == b[0]? b[1]-a[1]:a[0]-b[0];
            }
        });
        
        int[] height = new int[env.length];
        for(int i = 0; i < env.length; i++) height[i] = env[i][1];
        
        return longest_increase_sequence(height);
    }
    
    public int longest_increase_sequence(int[] nums){
        int[] dp = new int[nums.length];
        Arrays.fill(dp,1);
        
        int max = 0;
        for(int i = 0; i < nums.length; i++){
            for(int j = 0; j < i; j++){
                if(nums[i] > nums[j]) dp[i] = Math.max(dp[i],dp[j] + 1);
            }
            max = Math.max(max,dp[i]);
        }
        return max;
    }
}
```

Patience Sorting (Binary Search): Beat 100%
```
class Solution {
    public int maxEnvelopes(int[][] env) {
        Arrays.sort(env,new Comparator<>(){
            public int compare(int[] a, int[] b){
                return a[0] == b[0]? b[1]-a[1]:a[0]-b[0];
            }
        });
        
        int[] height = new int[env.length];
        for(int i = 0; i < env.length; i++) height[i] = env[i][1];
        
        return longest_increase_sequence(height);
    }
    
    public int longest_increase_sequence(int[] nums){
        int[] piles = new int[nums.length];
        int index = 0;
        
        for(int i = 0; i < nums.length; i++){
            int next = find_insert_postion(piles,index,nums[i]);
            piles[next] = nums[i];
            if(next == index) index++;
        }
        
        return index;
    }
    
//     equal to or slightly larger
    public int find_insert_postion(int[] nums, int index, int target){
        int start = 0, end = index;
        while(start < end){
            int mid = (start + end) >>> 1;
            if(nums[mid] == target) return mid;
            if(nums[mid] < target) start = mid + 1;
            else end = mid;
        }
        return start;
    }
}
```

# Solution 2: Direct DP (Beat 42%)
```
class Solution {
    public int maxEnvelopes(int[][] env) {
        Arrays.sort(env,new Comparator<>(){
            public int compare(int[] a, int[] b){
                return a[0] == b[0]? a[1]-b[1]:a[0]-b[0];
            }
        });
        
        int res = 0;
        int[] dp = new int[env.length];
        Arrays.fill(dp,1);
        
        for(int i = 0; i < env.length; i++){
            for(int j = 0; j < i; j++){
                if(env[i][0] > env[j][0] && env[i][1] > env[j][1]) dp[i] = Math.max(dp[i],dp[j]+1);
            }
            res = Math.max(res,dp[i]);
        }
        
        return res;
    }
    

}
```