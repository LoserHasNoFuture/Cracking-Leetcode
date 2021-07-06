# 532. K-diff Pairs in an Array
Given an array of integers  `nums`  and an integer  `k`, return  _the number of  **unique**  k-diff pairs in the array_.

A  **k-diff**  pair is an integer pair  `(nums[i], nums[j])`, where the following are true:

-   `0 <= i < j < nums.length`
-   `|nums[i] - nums[j]| == k`

**Notice**  that  `|val|`  denotes the absolute value of  `val`.

**Example 1:**

**Input:** nums = [3,1,4,1,5], k = 2
**Output:** 2
**Explanation:** There are two 2-diff pairs in the array, (1, 3) and (3, 5).
Although we have two 1s in the input, we should only return the number of **unique** pairs.

**Example 2:**

**Input:** nums = [1,2,3,4,5], k = 1
**Output:** 4
**Explanation:** There are four 1-diff pairs in the array, (1, 2), (2, 3), (3, 4) and (4, 5).

**Example 3:**

**Input:** nums = [1,3,1,5,4], k = 0
**Output:** 1
**Explanation:** There is one 0-diff pair in the array, (1, 1).

**Example 4:**

**Input:** nums = [1,2,4,4,3,3,0,9,2,3], k = 3
**Output:** 2

**Example 5:**

**Input:** nums = [-1,-2,-3], k = 1
**Output:** 2

**Constraints:**

-   `1 <= nums.length <= 104`
-   `-107  <= nums[i] <= 107`
-   `0 <= k <= 107`

# Solution 1: HashMap O(n) Space and O(n) Time
A variance of two sum
```
class Solution {
    public int findPairs(int[] nums, int k) {
        HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
        for(int num:nums){
            int cnt = map.getOrDefault(num,0) + 1;
            map.put(num,cnt);
        }
        
        if(k == 0) return num_of_duplicates(map);
        
        return num_of_pairs(k,map);
        
    }
    
    public int num_of_pairs(int k, HashMap<Integer,Integer> map){
        int res = 0;
        for(int num: map.keySet()){
            if(map.containsKey(num+k)) res++;
        }
        return res;
    }
    
    public int num_of_duplicates(HashMap<Integer,Integer> map){
        int res = 0;
        for(int num: map.keySet()){
            if(map.get(num) > 1) res++;
        }
        return res;
    }
    
}
```

# Solution 2: Two Pointer O(1) Space, O(nlogn) Time
```
class Solution {
    public int findPairs(int[] nums, int k) {
        if(nums.length <= 1) return 0;
        Arrays.sort(nums);
        int res = 0;
        
        for(int i = 0, j = 0; i < nums.length; i++){
            for(j = Math.max(j,i+1); j < nums.length && nums[j] - nums[i] < k; j++){
                if(nums[j] - nums[i] < 0) return res;
            }
            if(j < nums.length  && nums[j] - nums[i] == k) res++;
            while(i + 1 < nums.length && nums[i] == nums[i+1]) i++;
        }
        
        return res;
    }

}
```