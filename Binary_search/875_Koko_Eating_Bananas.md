# 875 Koko Eating Bananas
Koko loves to eat bananas. There are  `N` piles of bananas, the  `i`-th pile has  `piles[i]`  bananas. The guards have gone and will come back in  `H`  hours.

Koko can decide her bananas-per-hour eating speed of  `K`. Each hour, she chooses some pile of bananas, and eats K bananas from that pile. If the pile has less than  `K`  bananas, she eats all of them instead, and won't eat any more bananas during this hour.

Koko likes to eat slowly, but still wants to finish eating all the bananas before the guards come back.

Return the minimum integer  `K`  such that she can eat all the bananas within  `H`  hours.

**Example 1:**

**Input:** piles = [3,6,7,11], H = 8
**Output:** 4

**Example 2:**

**Input:** piles = [30,11,23,4,20], H = 5
**Output:** 30

**Example 3:**

**Input:** piles = [30,11,23,4,20], H = 6
**Output:** 23

**Constraints:**

-   `1 <= piles.length <= 10^4`
-   `piles.length <= H <= 10^9`
-   `1 <= piles[i] <= 10^9`


# Solution: Binary Search
```
class Solution {
    public int minEatingSpeed(int[] piles, int H) {
        if(piles.length == 0) return 0;
        int end = find_max_pile(piles);
        if(H == piles.length) return end;
        int start = 1;
        
        while(start < end){
            int mid = (start + end) >>> 1;
            if(can_eat_all(piles, mid, H)) end = mid;
            else start = mid + 1;
        }
        
        return start;
    }
    
    public boolean can_eat_all(int[] nums, int speed, int count){
        for(int i = 0; i<nums.length; i++) {
            count -= nums[i]%speed == 0? nums[i]/speed:nums[i]/speed + 1;
            if(count < 0) return false;
        }
        return true;
    }
    
    
    public int find_max_pile(int[] nums){
        int res = nums[0];
        for(int i = 1; i < nums.length; i++) res = Math.max(res, nums[i]);
        return res;
    }
}
```