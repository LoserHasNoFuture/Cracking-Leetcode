# 135. Candy
There are  `n`  children standing in a line. Each child is assigned a rating value given in the integer array  `ratings`.

You are giving candies to these children subjected to the following requirements:

-   Each child must have at least one candy.
-   Children with a higher rating get more candies than their neighbors.

Return  _the minimum number of candies you need to have to distribute the candies to the children_.

**Example 1:**

**Input:** ratings = [1,0,2]
**Output:** 5
**Explanation:** You can allocate to the first, second and third child with 2, 1, 2 candies respectively.

**Example 2:**

**Input:** ratings = [1,2,2]
**Output:** 4
**Explanation:** You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
The third child gets 1 candy because it satisfies the above two conditions.

**Constraints:**

-   `n == ratings.length`
-   `1 <= n <= 2 * 104`
-   `0 <= ratings[i] <= 2 * 104`

# Solution 1: O(n) Solution (Beat 100%)
Refer from: [https://leetcode.com/problems/candy/discuss/135698/Simple-solution-with-one-pass-using-O(1)-space](https://leetcode.com/problems/candy/discuss/135698/Simple-solution-with-one-pass-using-O(1)-space)
```
class Solution {
    
    
    public int candy(int[] nums) {
        if(nums.length <= 1) return nums.length;
        int res = 1, n = nums.length;
        int prev = 1; // the candy previous kid has
        int peak_candy = 1; // the peak before rating goes down, peak_candy is the number of candy it gets
        int down = 0; // the number of going down
        
        for(int i = 1; i < n; i++){
            if(nums[i-1] == nums[i]){
                res++;
                prev = 1;
                down = 0;
                peak_candy = prev;
            }else if (nums[i-1] > nums[i]){
                down++;
                if(down < peak_candy) {
                    res += down;
                }else{
                    res += (down + 1);
                    peak_candy++;
                }
                prev = 1;
            }else{
                res += (++prev);
                down = 0;
                peak_candy = prev;
            }
        }
        
        return res;
    }
}
```

Combine same code:
```
class Solution {
    public int candy(int[] nums) {
        if(nums.length <= 1) return nums.length;
        int res = 1, n = nums.length;
        int prev = 1; // the candy previous kid has
        int peak_candy = 1; // 
        int down = 0; // the number of going down
        
        for(int i = 1; i < n; i++){
            if(nums[i-1] <= nums[i]){
                if(nums[i-1] < nums[i]){
                    prev++;
                }else{
                    prev = 1;
                }
                down = 0;
                res += prev;
                candy = prev;
            }else if (nums[i-1] > nums[i]){
                down++;
                res += down;
                if(down >= candy) {
                    res++;
                    candy++;
                }
                prev = 1;
            }
        }
        
        return res;
    }
}
```


# Solution 2: Shitty O(nlogn) Solution (Beat 12%)
Sort before assigning:
```
class Solution {
    
    class Child{
        int rating;
        int index;
        
        public Child(int _rating, int _index){
            this.rating = _rating;
            this.index = _index;
        }
    }
    
    public int candy(int[] ratings) {
        if(ratings.length <= 1) return ratings.length;
        int res = 0, n = ratings.length;
        PriorityQueue<Child> pq = new PriorityQueue<Child>((a,b)->(a.rating-b.rating));
        
        int[] dp = new int[n];
        for(int i = 0; i < n; i++) pq.offer(new Child(ratings[i],i));
        
        while(!pq.isEmpty()){
            Child cur = pq.poll();
            if(cur.index == 0) {
                if(ratings[cur.index] == ratings[cur.index + 1]) dp[cur.index] = 1;
                else dp[cur.index] = dp[cur.index + 1] + 1;
            }
            else if(cur.index == n-1) {
                if(ratings[cur.index] == ratings[cur.index - 1]) dp[cur.index] = 1;
                else dp[cur.index] = dp[cur.index-1] + 1;
            }else if (dp[cur.index-1] == 0 && dp[cur.index+1] == 0) dp[cur.index] = 1;
            else if (dp[cur.index-1] > 0 && dp[cur.index+1] > 0){
                if(ratings[cur.index] == ratings[cur.index-1] && ratings[cur.index] == ratings[cur.index+1]) 
                    dp[cur.index] = 1;
                else if (ratings[cur.index] > ratings[cur.index-1] && ratings[cur.index] > ratings[cur.index+1])
                    dp[cur.index] = Math.max(dp[cur.index-1],dp[cur.index+1]) + 1;
                else{
                    if(ratings[cur.index] > ratings[cur.index - 1]){
                        dp[cur.index] = dp[cur.index - 1] + 1;
                    }else dp[cur.index] = dp[cur.index + 1] + 1;
                }
            }else if(dp[cur.index-1] > 0){
                if(ratings[cur.index-1] == ratings[cur.index]) dp[cur.index] = 1;
                else dp[cur.index] = dp[cur.index-1] + 1;
            }else {
                if(ratings[cur.index+1] == ratings[cur.index]) dp[cur.index] = 1;
                else dp[cur.index] = dp[cur.index+1] + 1;
            }
            
            res += dp[cur.index];
        }
        
        return res;
    }
}
```