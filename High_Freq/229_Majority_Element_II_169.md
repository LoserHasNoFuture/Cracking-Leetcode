# 229. Majority Element II 169
Given an integer array of size  `n`, find all elements that appear more than  `⌊ n/3 ⌋`  times.

**Example 1:**

**Input:** nums = [3,2,3]
**Output:** [3]

**Example 2:**

**Input:** nums = [1]
**Output:** [1]

**Example 3:**

**Input:** nums = [1,2]
**Output:** [1,2]

**Constraints:**

-   `1 <= nums.length <= 5 * 104`
-   `-109  <= nums[i] <= 109`

**Follow up:**  Could you solve the problem in linear time and in  `O(1)`  space?

# Solution 1: HashMap
count appearance frequency for all elements

# Solution 2: Boyer-Moore Vote Algo (Beat 78%)
```
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        List<Integer> res = new ArrayList<Integer>();
        int cnt1 = 0, cnt2 = 0, num1 = 0, num2 = 0;
        
        // the order of if-else is very crucial.
        for(int num: nums){
            if(num == num1) cnt1++;
            else if( num == num2) cnt2++;
            else if(cnt1 == 0){
                num1 = num;
                cnt1 = 1;
            }else if(cnt2 == 0){
                num2 = num;
                cnt2 = 1;
            }else{
                cnt1--;
                cnt2--;
            }
        }
        
        cnt1 = 0; cnt2 = 0;
        for(int num: nums){
            if(num == num1) cnt1++;
            // using else-if is necessary, consider the case.  [0,0,0]
            else if(num == num2) cnt2++; 
        }
        
        if(cnt1 > nums.length/3) res.add(num1);
        if(cnt2 > nums.length/3) res.add(num2);
        return res;
    }
}
```