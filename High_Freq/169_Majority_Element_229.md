# 169. Majority Element
Given an array of size  _n_, find the majority element. The majority element is the element that appears  **more than**  `⌊ n/2 ⌋`  times.

You may assume that the array is non-empty and the majority element always exist in the array.

**Example 1:**

**Input:** [3,2,3]
**Output:** 3

**Example 2:**

**Input:** [2,2,1,1,1,2,2]
**Output:** 2

# Solution 1: HashMap (Beat 13%)
```
class Solution {
    HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
    public int majorityElement(int[] nums) {
        for(int i = 0; i < nums.length; i++){
            int cnt = map.getOrDefault(nums[i],0) + 1;
            if(cnt > nums.length/2) return nums[i];
            map.put(nums[i],cnt);
        }
        return -1;
    }
}
```

# Solution 2: Smart Solution (Beat 100%)
Refer: [https://leetcode.com/problems/majority-element/discuss/51613/O(n)-time-O(1)-space-fastest-solution](https://leetcode.com/problems/majority-element/discuss/51613/O(n)-time-O(1)-space-fastest-solution)
```
class Solution {
    public int majorityElement(int[] nums) {
        int cur = 0, cnt = 0;
        
        for(int num: nums){
            if(cnt == 0) cur = num;
            if(cur == num) cnt++;
            else cnt--;
        }
        
        return cur;
    }
}
```

# Solution 3: Sort and Get the Middle One (Beat 100%)
```
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        return nums[n/2];
    }
}
```

# Solution 4: Divide and Conquer.  Find the n/2-th smallest element
Quick selection.