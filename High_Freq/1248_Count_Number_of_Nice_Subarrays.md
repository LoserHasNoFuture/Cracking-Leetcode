# 1248. Count Number of Nice Subarrays
Given an array of integers  `nums`  and an integer  `k`. A continuous subarray is called  **nice**  if there are  `k`  odd numbers on it.

Return  _the number of  **nice**  sub-arrays_.

**Example 1:**

**Input:** nums = [1,1,2,1,1], k = 3
**Output:** 2
**Explanation:** The only sub-arrays with 3 odd numbers are [1,1,2,1] and [1,2,1,1].

**Example 2:**

**Input:** nums = [2,4,6], k = 1
**Output:** 0
**Explanation:** There is no odd numbers in the array.

**Example 3:**

**Input:** nums = [2,2,2,1,2,2,1,2,2,2], k = 2
**Output:** 16

**Constraints:**

-   `1 <= nums.length <= 50000`
-   `1 <= nums[i] <= 10^5`
-   `1 <= k <= nums.length`

# Solution 1: HashMap (Beat 12%)
```
class Solution {
    public int numberOfSubarrays(int[] nums, int k) {
        HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
        map.put(0,1);
        int cnt = 0, res = 0;
        for(int num:nums){
            if(num%2 == 1) cnt++;
            int tmp = map.getOrDefault(cnt,0) + 1;
            map.put(cnt,tmp);
            if(map.containsKey(cnt-k)) res += map.get(cnt-k) ;
        }
        return res;
    }
}
```

# Solution 2: Sliding Window (Beat 90%)
```
class Solution {
    public int numberOfSubarrays(int[] nums, int k) {
        int cnt = 0, res = 0, left = 0, right = 0, variations = 0;
        
        while(right < nums.length){
            if(nums[right++]%2 == 1) {
                cnt++;
                int tmp = 0;
                while(cnt == k){
                    tmp++;
                    if(nums[left++]%2 == 1) cnt--;
                }
                res += tmp;
                variations = tmp;
            }else res += variations;
        }
        
        return res;
    }
}
```

# Solution 3: Sliding Window (Similar to 992)
```
class Solution {
    public int numberOfSubarrays(int[] nums, int k) {
        return at_most(k,nums) - at_most(k-1,nums);
    }
    
    public int at_most(int k, int[] nums){
        int res = 0, left = 0, right = 0, n = nums.length, valid = 0;
        
        while(right < n){
            int num = nums[right];
            if(num%2 == 1) valid++;
            
            while(valid > k){
                if(nums[left++]%2 == 1) valid--;
            }
            
            // number of subarrays end at right
            res += right - left + 1;
            right++;
        }
        
        return res;
    }
}
```