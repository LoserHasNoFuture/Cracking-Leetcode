# 473. Matchsticks to Square
Remember the story of Little Match Girl? By now, you know exactly what matchsticks the little match girl has, please find out a way you can make one square by using up all those matchsticks. You should not break any stick, but you can link them up, and each matchstick must be used  **exactly**  one time.

Your input will be several matchsticks the girl has, represented with their stick length. Your output will either be true or false, to represent whether you could make one square using all the matchsticks the little match girl has.

**Example 1:**  

**Input:** [1,1,2,2,2]
**Output:** true

**Explanation:** You can form a square with length 2, one side of the square came two sticks with length 1.

**Example 2:**  

**Input:** [3,3,3,3,4]
**Output:** false

**Explanation:** You cannot find a way to form a square with all the matchsticks.

**Note:**  

1.  The length sum of the given matchsticks is in the range of  `0`  to  `10^9`.
2.  The length of the given matchstick array will not exceed  `15`.


# Solution 1: Traditional BackTracking (Beat 45%)
```
class Solution {
    boolean exceed = false;
    public boolean makesquare(int[] nums) {
        if(nums.length < 4) return false;
        int sum = 0;
        for(int num: nums) sum += num;
        if(sum % 4 != 0) return false;
        
        int len = sum/4;
        
        boolean[] visited = new boolean[nums.length];
        int tmp = -1;
        visited[0] = true;
        return dfs(nums,visited,len,len,4,0);
    }
    
    public boolean dfs(int[] nums, boolean[] visited, int len, int sum, int count, int index){
        if(nums[index] > len){
            exceed = true;
            return false;
        }
        if(sum - nums[index] == 0) {
            count--;
            sum = len;
            if(count == 0) return true;
        }else sum -= nums[index];
        
        int tmp = -1;
        for(int j = 0; j < nums.length; j++){
            if(exceed) return false;
            if(visited[j] || nums[j] > sum || nums[j] == tmp) continue;
            visited[j] = true;
            if(dfs(nums,visited,len,sum,count,j)) return true;
            visited[j] = false;
            tmp = nums[j];
        }
        
        return false;
    }
    
}
```

# Solution 2: Optimization (Beat 60%)
1. Consider why we sort nums and the index is decreasing.
2. Why we iterate s instead of nums in makesquareSub

```
public class Solution {
    public boolean makesquare(int[] nums) {
        if (nums.length < 4) return false;

        int perimeter = 0;
        for (int ele : nums) perimeter += ele;
        if (perimeter % 4 != 0) return false;

        Arrays.sort(nums);
        int side = perimeter / 4;

        return makesquareSub(nums, nums.length - 1, new int[] {side, side, side, side});
    }

    private boolean makesquareSub(int[] nums, int i, int[] s) {
        if (i < 0) return s[0] == 0 && s[1] == 0 && s[2] == 0 && s[3] == 0;

        for (int j = 0; j < s.length; j++) {
            if (nums[i] > s[j]) continue;
            s[j] -= nums[i];
            if (makesquareSub(nums, i - 1, s)) return true;
            s[j] += nums[i];
        }

        return false;
    }

}
```


# Solution 3: DP
To be continued