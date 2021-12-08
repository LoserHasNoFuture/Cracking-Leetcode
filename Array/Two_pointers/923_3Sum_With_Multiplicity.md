# 923. 3Sum With Multiplicity
Given an integer array  `arr`, and an integer  `target`, return the number of tuples  `i, j, k`  such that  `i < j < k`  and  `arr[i] + arr[j] + arr[k] == target`.

As the answer can be very large, return it  **modulo**  `109  + 7`.

**Example 1:**

**Input:** arr = [1,1,2,2,3,3,4,4,5,5], target = 8
**Output:** 20
**Explanation:** 
Enumerating by the values (arr[i], arr[j], arr[k]):
(1, 2, 5) occurs 8 times;
(1, 3, 4) occurs 8 times;
(2, 2, 4) occurs 2 times;
(2, 3, 3) occurs 2 times.

**Example 2:**

**Input:** arr = [1,1,2,2,2,2], target = 5
**Output:** 12
**Explanation:** 
arr[i] = 1, arr[j] = arr[k] = 2 occurs 12 times:
We choose one 1 from [1,1] in 2 ways,
and two 2s from [2,2,2,2] in 6 ways.

**Constraints:**

-   `3 <= arr.length <= 3000`
-   `0 <= arr[i] <= 100`
-   `0 <= target <= 300`

# Solution 1: Similar to 3-Sum
```
class Solution {
    int MOD = 1000000007, res = 0, r = 0, n = 0;
    public int threeSumMulti(int[] arr, int target) {
        if(arr.length < 3) return res;
        Arrays.sort(arr);
        
        n = arr.length;
        r = n-1;
        for(int i = 0; i < n - 2; i++){
            two_sum(arr, i+1 ,target-arr[i]);
        }
        
        return res;
    }
    
    public void two_sum(int[] nums, int l, int target){
        int left = l, right = nums.length - 1; 
        int ori_r = r;
        r = 0;
        
        while(left < right){
            if(nums[left] + nums[right] == target){
                r = Math.max(r, right);
                int tmp = 0;
                if(nums[left] != nums[right]) {
                    int tmp_l = left, tmp_r = right;
                    while(tmp_l < tmp_r && nums[left] == nums[tmp_l]) tmp_l++;
                    while(nums[right] == nums[tmp_r]) tmp_r--;
                    tmp = ((tmp_l - left) * (right - tmp_r)) % MOD;
                    left = tmp_l;
                    right = tmp_r;
                }else{
                    int cnt = right - left;
                    tmp = (cnt*(cnt + 1)/2)%MOD;
                    res = (res + tmp) % MOD;   
                    break;
                }
                res = (res + tmp)%MOD;
                
            }else if(nums[left] + nums[right] > target) right--;
            else left++;
        }
        
        if(r == 0) r = ori_r;
    }
}
```

# Solution 2: Using bucket
易错点: 考虑三个不一样的书,两个不一样的数.三个数都不一样的情况
Refer from: https://leetcode.com/problems/3sum-with-multiplicity/discuss/181131/C%2B%2BJavaPython-O(N-%2B-101-*-101)
```
class Solution {
    public int threeSumMulti(int[] A, int target) {
        long[] c = new long[101];
        for (int a : A) c[a]++;
        long res = 0;
        for (int i = 0; i <= 100; i++)
            for (int j = i; j <= 100; j++) {
                int k = target - i - j;
                if (k > 100 || k < 0) continue;
                if (i == j && j == k)
                    res += c[i] * (c[i] - 1) * (c[i] - 2) / 6;
                else if (i == j && j != k)
                    res += c[i] * (c[i] - 1) / 2 * c[k];
                else if (j < k)
                    res += c[i] * c[j] * c[k];
            }
        return (int)(res % (1e9 + 7));
    }

}
```
