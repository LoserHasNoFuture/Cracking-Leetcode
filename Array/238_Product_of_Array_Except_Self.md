# 238. Product of Array Except Self
Given an array  `nums`  of  _n_  integers where  _n_  > 1, return an array  `output`  such that  `output[i]`  is equal to the product of all the elements of  `nums`  except  `nums[i]`.

**Example:**

**Input:**  `[1,2,3,4]`
**Output:** `[24,12,8,6]`

**Constraint:** It's guaranteed that the product of the elements of any prefix or suffix of the array (including the whole array) fits in a 32 bit integer.

**Note:** Please solve it  **without division**  and in O(_n_).

**Follow up:**  
Could you solve it with constant space complexity? (The output array  **does not**  count as extra space for the purpose of space complexity analysis.)

# Solution 1: Using Division (Beat 100%)
**0** is the corner case.
```
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int all = 1;
        boolean hasZero = false;
        boolean hasTwoZeros = false;
        for(int num: nums) {
            if(num != 0) all *= num;
            else if(!hasZero) hasZero = true;
            else hasTwoZeros = true;
        }
        
        int[] res = new int[nums.length];
        if(hasTwoZeros) return res;
        
        for(int i = 0; i < nums.length; i++) {
            if(nums[i] == 0) res[i] = all;
            else if (!hasZero) res[i] = all/nums[i];
            else res[i] = 0;
        }
        
        return res;
    }
}
```

# Solution 2: Withou Dividend (Beat 100%)
Refer to: https://leetcode.com/problems/product-of-array-except-self/discuss/65622/Simple-Java-solution-in-O(n)-without-extra-space
"
Given numbers  `[2, 3, 4, 5]`, regarding the third number 4, the product of array except 4 is  `2*3*5`  which consists of two parts: left  `2*3`  and right  `5`. The product is  `left*right`. We can get lefts and rights:

```
Numbers:     2    3    4     5
Lefts:            2  2*3 2*3*4
Rights:  3*4*5  4*5    5      

```

Let’s fill the empty with 1:

```
Numbers:     2    3    4     5
Lefts:       1    2  2*3 2*3*4
Rights:  3*4*5  4*5    5     1

```

We can calculate lefts and rights in 2 loops. The time complexity is O(n).
"
```
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] res = new int[nums.length];
        res[0] = 1;
        for(int i = 1; i < nums.length; i++) res[i] = res[i-1] * nums[i-1];
        
        int tmp = 1;
        for(int i = nums.length - 2; i >= 0; i--){
            tmp = tmp * nums[i+1];
            res[i] *= tmp;
        }
        
        return res;
    }
}
```

