# 823. Binary Trees With Factors
Given an array of unique integers,  `arr`, where each integer  `arr[i]`  is strictly greater than  `1`.

We make a binary tree using these integers, and each number may be used for any number of times. Each non-leaf node's value should be equal to the product of the values of its children.

Return  _the number of binary trees we can make_. The answer may be too large so return the answer  **modulo**  `109  + 7`.

**Example 1:**

**Input:** arr = [2,4]
**Output:** 3
**Explanation:** We can make these trees: `[2], [4], [4, 2, 2]`

**Example 2:**

**Input:** arr = [2,4,5,10]
**Output:** 7
**Explanation:** We can make these trees: `[2], [4], [5], [10], [4, 2, 2], [10, 2, 5], [10, 5, 2]`.

**Constraints:**

-   `1 <= arr.length <= 1000`
-   `2 <= arr[i] <= 109`
-   All the values of  `arr`  are  **unique**.


# Solution 1: DP (Beat 50%)
```
class Solution {
    public int numFactoredBinaryTrees(int[] arr) {
        HashMap<Integer,Long> map = new HashMap<Integer,Long>();
        Arrays.sort(arr);
        long res = 0L, mod = (long)1e9 + 7;
        
        for(int i = 0; i < arr.length; i++){
            long tmp = 1;
            for(int j = 0; j < i; j++){
                if(arr[i]%arr[j] == 0 && map.containsKey(arr[i]/arr[j]) && map.containsKey(arr[j])){
                    tmp = tmp + (map.get(arr[j]) * map.get(arr[i]/arr[j]));
                }
            }
            res = (res + tmp) % mod;
            map.put(arr[i],tmp);
        }
        
        return (int) res;
    }
}
```