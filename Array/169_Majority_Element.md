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
public class Solution {
    public int majorityElement(int[] num) {

        int major=num[0], count = 1;
        for(int i=1; i<num.length;i++){
            if(count==0){
                count++;
                major=num[i];
            }else if(major==num[i]){
                count++;
            }else count--;
            
        }
        return major;
    }
}
```