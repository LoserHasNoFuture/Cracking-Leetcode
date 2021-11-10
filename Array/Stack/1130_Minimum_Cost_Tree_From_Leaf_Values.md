# 1130. Minimum Cost Tree From Leaf Values
Given an array  `arr`  of positive integers, consider all binary trees such that:

-   Each node has either  `0`  or  `2`  children;
-   The values of  `arr`  correspond to the values of each  **leaf**  in an in-order traversal of the tree.
-   The value of each non-leaf node is equal to the product of the largest leaf value in its left and right subtree, respectively.

Among all possible binary trees considered, return  _the smallest possible sum of the values of each non-leaf node_. It is guaranteed this sum fits into a  **32-bit**  integer.

A node is a  **leaf**  if and only if it has zero children.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/08/10/tree1.jpg)

**Input:** arr = [6,2,4]
**Output:** 32
**Explanation:** There are two possible trees shown.
The first has a non-leaf node sum 36, and the second has non-leaf node sum 32.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/08/10/tree2.jpg)

**Input:** arr = [4,11]
**Output:** 44

**Constraints:**

-   `2 <= arr.length <= 40`
-   `1 <= arr[i] <= 15`
-   It is guaranteed that the answer fits into a  **32-bit**  signed integer (i.e., it is less than 231).


# Solution 1: Using DFS O(nlogn)
```
class Solution {
    int res = 0;
    public int mctFromLeafValues(int[] arr) {
        if(arr.length == 0) return 0;
        if(arr.length == 1) return arr[0];
        dfs(arr,0,arr.length - 1);
        return res;
    }
    
    public int dfs(int[] arr, int l, int r){
        if(l > r) return 0;
        if(l == r) return arr[l];
        
        int mid = find_max(arr,l,r);
        int left = dfs(arr,l, mid - 1);
        int right = dfs(arr, mid + 1, r);
        res = (left + right) * arr[mid] + res;
        
        return arr[mid];
    }
    
    public int find_max(int[] arr, int l, int r){
        int max = arr[l];
        int index = l;
        for(int i = l+1; i <= r; i++){
            if(arr[i] > max){
                max = arr[i];
                index = i;
            }
        }
        
        return index;
    }
}
```

# Solution 2: Using Stack O(n)
```
class Solution {
    public int mctFromLeafValues(int[] arr) {
        int res = 0, n = arr.length, index = -1;
        if(n == 0) return res;
        int[] stack = new int[n];
        
        for(int num: arr){
            while(index >= 0 && num >= stack[index]){
                int tmp = stack[index--];
                if(index >= 0 && num >= stack[index]) res += stack[index] * tmp;
                else res += num * tmp;
            }
            stack[++index] = num;
        }
        
        while(index >= 1) res += stack[index--]  * stack[index];   
        return res;
    }
    
}
```