# 740. Delete and Earn
Given an array  `nums`  of integers, you can perform operations on the array.

In each operation, you pick any  `nums[i]`  and delete it to earn  `nums[i]`  points. After, you must delete  **every**  element equal to  `nums[i] - 1`  or  `nums[i] + 1`.

You start with  `0`  points. Return the maximum number of points you can earn by applying such operations.

**Example 1:**

**Input:** nums = [3,4,2]
**Output:** 6
**Explanation:** Delete 4 to earn 4 points, consequently 3 is also deleted.
Then, delete 2 to earn 2 points.
6 total points are earned.

**Example 2:**

**Input:** nums = [2,2,3,3,3,4]
**Output:** 9
**Explanation:** Delete 3 to earn 3 points, deleting both 2's and the 4.
Then, delete 3 again to earn 3 points, and 3 again to earn 3 points.
9 total points are earned.

**Constraints:**

-   `1 <= nums.length <= 2 * 104`
-   `1 <= nums[i] <= 104`

# Solution 1: Using Bucket and DP (Beat 100%)
```
class Solution {
    public int deleteAndEarn(int[] nums) {
        int[] buckets = new int[10001];
        for (int num : nums) {
            buckets[num] += num;
        }
        int[] dp = new int[10001];
        dp[0] = buckets[0];
        dp[1] = buckets[1];
        for (int i = 2; i < buckets.length; i++) {
            dp[i] = Math.max(buckets[i] + dp[i - 2], dp[i - 1]);
        }
        return dp[10000];
    }
}
```

# Solution 2: Recursion+DP (Beat 100%)
This solution can be adapted to the case that ```num[i]``` is large.
```
class Solution {

    HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
    public int deleteAndEarn(int[] nums) {
        Arrays.sort(nums);
        
        return dfs(nums,0);
    }
    
    public int dfs(int[] nums, int index){
        if(index >= nums.length) return 0;
        if(map.containsKey(index)) return map.get(index);
        int cnt = 0, res = 0, ori_index = index;
        while(index + cnt < nums.length && nums[index] == nums[index+cnt]) cnt++;
        res += (nums[index] * cnt);
        
        if(index + cnt < nums.length && nums[index] + 1== nums[index+cnt]){
            index = index + cnt; cnt = 0;
            while(index + cnt < nums.length && nums[index] == nums[index+cnt]) cnt++;
            res = res + dfs(nums,index+cnt);
            
            int res2 = (nums[index]*cnt);
            if(index + cnt < nums.length && nums[index] + 1== nums[index+cnt]){
                index = index + cnt; cnt = 0;
                while(index + cnt < nums.length && nums[index] == nums[index+cnt]) cnt++;    
                res2 += dfs(nums,index+cnt);
            }else res2 = res2 + dfs(nums,index+cnt);
            
            res = Math.max(res,res2);
        }else res = res + dfs(nums,index+cnt);
        
        map.put(ori_index,res);
        return res;        
    }
    
}
```

Better Coding:
```
class Solution {
    public int deleteAndEarn(int[] nums) {
        Arrays.sort(nums);
        ArrayList<int[]> arr = new ArrayList<int[]>();
        add_nums_to_arr(nums,arr);
        
        int[] dp = new int[arr.size()+1];
        dp[1] = arr.get(0)[1];
        for(int i = 1;  i < arr.size(); i++){
            if(arr.get(i)[0] == arr.get(i-1)[0] + 1){
                dp[i+1] = Math.max(dp[i], dp[i-1]+arr.get(i)[1]);
            }else dp[i+1] = dp[i] + arr.get(i)[1];
        }
        return dp[arr.size()];
    }
    
    public void add_nums_to_arr(int[] nums, ArrayList<int[]> arr){
        int index = 0;
        while(index < nums.length){
            int cnt = 0;
            while(index + cnt < nums.length && nums[index] == nums[index+cnt]) cnt++;
            int sum = cnt*nums[index];
            arr.add(new int[]{nums[index],sum});
            index = index + cnt;
        }
    }
}
```