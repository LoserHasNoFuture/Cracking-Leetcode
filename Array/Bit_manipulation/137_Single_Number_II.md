# 137. Single Number II
Given an integer array  `nums`  where every element appears  **three times**  except for one, which appears  **exactly once**.  _Find the single element and return it_.

You must implement a solution with a linear runtime complexity and use only constant extra space.

**Example 1:**

**Input:** nums = [2,2,3,2]
**Output:** 3

**Example 2:**

**Input:** nums = [0,1,0,1,0,1,99]
**Output:** 99

**Constraints:**

-   `1 <= nums.length <= 3 * 104`
-   `-231  <= nums[i] <= 231  - 1`
-   Each element in  `nums`  appears exactly  **three times**  except for one element which appears  **once**.


# Solution: Bit Manipulation
```
class Solution {
    public int singleNumber(int[] nums) {
        int[] map = new int[32];
        for(int num: nums) put_into_map(num, map);
        for(int i = 0; i < 32; i++) map[i] = map[i]%3;
        return reconstruct_num_from(map);
    }
    
    public void put_into_map(int num, int[] map){
        for(int i = 0; i < 32; i++){
            if((num&1) == 1) map[i]++;
            num = num >> 1;
        }
    }
    
    public int reconstruct_num_from(int[] map){
        int res = 0;
        for(int i = 31; i >= 0; i--){
            res = res << 1;
            res += map[i];
        }
        return res;
    }
}
```