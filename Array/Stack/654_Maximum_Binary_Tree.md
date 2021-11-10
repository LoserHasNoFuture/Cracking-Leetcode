# 654. Maximum Binary Tree
You are given an integer array  `nums`  with no duplicates. A  **maximum binary tree**  can be built recursively from  `nums`  using the following algorithm:

1.  Create a root node whose value is the maximum value in  `nums`.
2.  Recursively build the left subtree on the  **subarray prefix**  to the  **left**  of the maximum value.
3.  Recursively build the right subtree on the  **subarray suffix**  to the  **right**  of the maximum value.

Return  _the  **maximum binary tree**  built from_ `nums`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/24/tree1.jpg)

**Input:** nums = [3,2,1,6,0,5]
**Output:** [6,3,5,null,2,0,null,null,1]
**Explanation:** The recursive calls are as follow:
- The largest value in [3,2,1,6,0,5] is 6. Left prefix is [3,2,1] and right suffix is [0,5].
    - The largest value in [3,2,1] is 3. Left prefix is [] and right suffix is [2,1].
        - Empty array, so no child.
        - The largest value in [2,1] is 2. Left prefix is [] and right suffix is [1].
            - Empty array, so no child.
            - Only one element, so child is a node with value 1.
    - The largest value in [0,5] is 5. Left prefix is [0] and right suffix is [].
        - Only one element, so child is a node with value 0.
        - Empty array, so no child.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/24/tree2.jpg)

**Input:** nums = [3,2,1]
**Output:** [3,null,2,null,1]

**Constraints:**

-   `1 <= nums.length <= 1000`
-   `0 <= nums[i] <= 1000`
-   All integers in  `nums`  are  **unique**.


# Solution 1: Recursion (O(nlogn) Beat 87%)
```
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return dfs(nums,0,nums.length-1);
    }
    
    public TreeNode dfs(int[] nums, int start, int end){
        if(start > end) return null;
        int index = find_max(nums,start,end);
        
        TreeNode root = new TreeNode(nums[index]);
        root.left = dfs(nums,start,index-1);
        root.right = dfs(nums,index+1,end);
        
        return root;
    }
    
    public int find_max(int[] nums, int start, int end){
        int max = start;
        
        for(int i = start+1; i <= end; i++){
            if(nums[i] > nums[max]) max = i;
        }
        
        return max;
    }
}
```


# Solution 2: Using Stack (O(n), Beat 80%)
```
class Solution {
    
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        Deque<TreeNode> stack = new ArrayDeque<TreeNode>();
        TreeNode head = new TreeNode(1001);
        stack.push(head);
        
        for(int num: nums){
            TreeNode cur = new TreeNode(num);
            while(!stack.isEmpty() && stack.peek().val < num) cur.left = stack.pop();
            
            if(!stack.isEmpty()) stack.peek().right = cur;
            stack.push(cur);
        }
        
        return head.right;
    }
}
```