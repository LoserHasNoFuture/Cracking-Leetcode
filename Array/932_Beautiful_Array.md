# 932. Beautiful Array
For some fixed  `n`, an array  `nums`  is  _beautiful_  if it is a permutation of the integers  `1, 2, ..., n`, such that:

For every  `i < j`, there is  **no** `k`  with  `i < k < j` such that  `nums[k] * 2 = nums[i] + nums[j]`.

Given  `n`, return  **any**  beautiful array  `nums`. (It is guaranteed that one exists.)

**Example 1:**

**Input:** n = 4
**Output:** [2,1,4,3]

**Example 2:**

**Input:** n = 5
**Output:** [3,1,2,5,4]

**Note:**

-   `1 <= n <= 1000`

# Solution: Odd + Even Pattern
Ref: [https://leetcode.com/problems/beautiful-array/discuss/186679/Odd-%2B-Even-Pattern-O(N)](https://leetcode.com/problems/beautiful-array/discuss/186679/Odd-%2B-Even-Pattern-O(N))
# **Intuition**:

Try to divide and conquer,  
so we have left part, right part.

One way is to divide into [1, N / 2] and [N / 2 + 1, N].  
But it will cause problems when we merge them.

Another way is to divide into odds part and evens part.  
So there is no  `k`  with  `A[k] * 2 = odd + even`

I brute force all permutations when N = 5:  
20 beautiful array found,  
only 4 don't fit odd + even pattern:  
`[2, 1, 4, 5, 3]`  
`[3, 1, 2, 5, 4]`  
`[3, 5, 4, 1, 2]`  
`[4, 5, 2, 1, 3]`  
  

# **Beautiful Array Properties**

Saying that an array is beautiful,  
there is no  `i < k < j`,  
such that  `A[k] * 2 = A[i] + A[j]`

Apply these 3 following changes a beautiful array,  
we can get a new beautiful array  
  

**1. Deletion**  
Easy to prove.

**2. Addition**  
If we have  `A[k] * 2 != A[i] + A[j]`,  
`(A[k] + x) * 2 = A[k] * 2 + 2x != A[i] + A[j] + 2x = (A[i] + x) + (A[j] + x)`

E.g:  `[1,3,2] + 1 = [2,4,3]`.

**3. Multiplication**  
If we have  `A[k] * 2 != A[i] + A[j]`,  
for any  `x != 0`,  
`(A[k] * x) * 2 = A[k] * 2 * x != (A[i] + A[j]) * x = (A[i] * x) + (A[j] * x)`

E.g:  `[1,3,2] * 2 = [2,6,4]`  
  

# **Explanation**

With the observations above, we can easily construct any beautiful array.  
Assume we have a beautiful array  `A`  with length  `N`

`A1 = A * 2 - 1`  is beautiful with only odds from  `1`  to  `N * 2 -1`  
`A2 = A * 2`  is beautiful with only even from  `2`  to  `N * 2`  
`B = A1 + A2`  beautiful array with length  `N * 2`

E.g:

```
A = [2, 1, 4, 5, 3]
A1 = [3, 1, 7, 9, 5]
A2 = [4, 2, 8, 10, 6]
B = A1 + A2 = [3, 1, 7, 9, 5, 4, 2, 8, 10, 6]

```

  

# **Time Complexity**:

I have iteration version here  `O(N)`    (**级数求和: [https://blog.csdn.net/algzjh/article/details/82533996](https://blog.csdn.net/algzjh/article/details/82533996)**)
Naive recursion is  `O(NlogN)`  
Recursion with one call or with cache is  `O(N)`

```
class Solution {
    public int[] beautifulArray(int N) {
        ArrayList<Integer> res = new ArrayList<>();
        res.add(1);
        while (res.size() < N) {
            ArrayList<Integer> tmp = new ArrayList<>();
            for (int i : res) if (i * 2 - 1 <= N) tmp.add(i * 2 - 1);
            for (int i : res) if (i * 2 <= N) tmp.add(i * 2);
            res = tmp;
        }
        return res.stream().mapToInt(i -> i).toArray();
    }
}
```
