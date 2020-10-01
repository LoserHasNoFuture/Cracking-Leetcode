# 1470. Shuffle the Array
Given the array  `nums`  consisting of  `2n`  elements in the form  `[x1,x2,...,xn,y1,y2,...,yn]`.

_Return the array in the form_  `[x1,y1,x2,y2,...,xn,yn]`.

**Example 1:**

**Input:** nums = [2,5,1,3,4,7], n = 3
**Output:** [2,3,5,4,1,7] 
**Explanation:** Since x1=2, x2=5, x3=1, y1=3, y2=4, y3=7 then the answer is [2,3,5,4,1,7].

**Example 2:**

**Input:** nums = [1,2,3,4,4,3,2,1], n = 4
**Output:** [1,4,2,3,3,2,4,1]

**Example 3:**

**Input:** nums = [1,1,2,2], n = 2
**Output:** [1,2,1,2]

**Constraints:**

-   `1 <= n <= 500`
-   `nums.length == 2n`
-   `1 <= nums[i] <= 10^3`

# Solution 1: 
Inspired by Lance Wang
```
class Solution {
    public int[] shuffle(int[] nums, int n) {
        for(int i = 0; i < n; i++)
            nums[i] = nums[i] + nums[n+i]*1001;
        
        for(int i = n; i > 0; i--){
            nums[2*i-1] = nums[i-1]/1001;
            nums[2*i-2] = nums[i-1]%1001;
        }
        
        return nums;
    }
}
```

# Solution 2: 
Same idea, but using bit manipulation.
[https://leetcode.com/problems/shuffle-the-array/discuss/675956/In-Place-O(n)-Time-O(1)-Space-With-Explanation-and-Analysis](https://leetcode.com/problems/shuffle-the-array/discuss/675956/In-Place-O(n)-Time-O(1)-Space-With-Explanation-and-Analysis)
```
public int[] shuffle(int[] nums, int n) {
    for (int i = 0; i < n; ++i) {
        nums[i + n] |= (nums[i] << 10);
    }
    for (int i = 0; i < n; ++i) {
        nums[i * 2] = (nums[i + n] & 0xFFC00) >> 10;     // 11111111110000000000 == 0xFFC00
        nums[i * 2 + 1] = nums[i + n] & 0x003FF;        // 00000000001111111111 == 0x003FF
    }
    return nums;
}
```

