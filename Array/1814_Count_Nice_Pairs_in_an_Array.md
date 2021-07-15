# 1814. Count Nice Pairs in an Array
You are given an array  `nums`  that consists of non-negative integers. Let us define  `rev(x)`  as the reverse of the non-negative integer  `x`. For example,  `rev(123) = 321`, and  `rev(120) = 21`. A pair of indices  `(i, j)`  is  **nice**  if it satisfies all of the following conditions:

-   `0 <= i < j < nums.length`
-   `nums[i] + rev(nums[j]) == nums[j] + rev(nums[i])`

Return  _the number of nice pairs of indices_. Since that number can be too large, return it  **modulo**  `109  + 7`.

**Example 1:**

**Input:** nums = [42,11,1,97]
**Output:** 2
**Explanation:** The two pairs are:
 - (0,3) : 42 + rev(97) = 42 + 79 = 121, 97 + rev(42) = 97 + 24 = 121.
 - (1,2) : 11 + rev(1) = 11 + 1 = 12, 1 + rev(11) = 1 + 11 = 12.

**Example 2:**

**Input:** nums = [13,10,35,24,76]
**Output:** 4

**Constraints:**

-   `1 <= nums.length <= 105`
-   `0 <= nums[i] <= 109`

# Solution 1: HashMap (Beat 86%)
Ref: [https://leetcode.com/problems/count-nice-pairs-in-an-array/discuss/1140639/JavaC%2B%2BPython-Straight-Forward](https://leetcode.com/problems/count-nice-pairs-in-an-array/discuss/1140639/JavaC%2B%2BPython-Straight-Forward)

`A[i] + rev(A[j]) == A[j] + rev(A[i])`  
`A[i] - rev(A[i]) == A[j] - rev(A[j])`  
`B[i] = A[i] - rev(A[i])`

Then it becomes an easy question that,  
how many pairs in B with  `B[i] == B[j]`  
  
Time  `O(nloga)`  
Space  `O(n)`

```
class Solution {
    public int countNicePairs(int[] A) {
        int res = 0, mod = (int)1e9 + 7;
        Map<Integer, Integer> count = new HashMap<>();;
        for (int a : A) {
            int b = reverse(a), v = count.getOrDefault(a - b, 0);
            count.put(a - b, v + 1);
            res = (res + v) % mod;
        }
        return res;
    }
    
    
    public int reverse(int num){
        int rev = 0;
        while(num > 0){
            rev = rev * 10 + num%10;
            num = num/10;
        }
        return rev;
    }
}
```