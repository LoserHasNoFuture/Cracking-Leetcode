# 536. Construct Binary Tree from String
You need to construct a binary tree from a string consisting of parenthesis and integers.

The whole input represents a binary tree. It contains an integer followed by zero, one or two pairs of parenthesis. The integer represents the root's value and a pair of parenthesis contains a child binary tree with the same structure.

You always start to construct the  **left**  child node of the parent first if it exists.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/02/butree.jpg)

**Input:** s = "4(2(3)(1))(6(5))"
**Output:** [4,2,6,3,1,5]

**Example 2:**

**Input:** s = "4(2(3)(1))(6(5)(7))"
**Output:** [4,2,6,3,1,5,7]

**Example 3:**

**Input:** s = "-4(2(3)(1))(6(5)(7))"
**Output:** [-4,2,6,3,1,5,7]

**Constraints:**

-   `0 <= s.length <= 3 * 104`
-   `s`  consists of digits,  `'('`,  `')'`, and  `'-'`  only.

# Solution: Using Stack
```
class Solution {
    public TreeNode str2tree(String s) {
        if(s.length() == 0) return null;
        Deque<TreeNode> stack = new ArrayDeque<>();
        int index = 0;
        int next = find_next_node(s,index);
        
        TreeNode cur = new TreeNode(Integer.parseInt(s.substring(index, next)));
        stack.push(cur);
        
        while(next < s.length()){
            index = next+1;
            next = find_next_node(s,index);
            cur = new TreeNode(Integer.parseInt(s.substring(index, next)));
            if(stack.peek().left == null) stack.peek().left = cur;
            else stack.peek().right = cur;
            stack.push(cur);
            
            while(next < s.length() && s.charAt(next) == ')'){
                stack.pop();
                next++;
            }
        }
        
        return stack.pop();
    }
    
    
    
    public int find_next_node(String s, int index){
        while(index < s.length() && s.charAt(index) != '(' && s.charAt(index) != ')') index++;
        return index;
    }
    
}
```

Using Array to mock Stack
```
class Solution {
    public TreeNode str2tree(String s) {
        if(s.length() == 0) return null;
        TreeNode[] stack = new TreeNode[s.length()];
        int index = 0, head = 0;
        int next = find_next_node(s,index);
        
        TreeNode cur = new TreeNode(Integer.parseInt(s.substring(index, next)));
        stack[head++] = cur;
        
        while(next < s.length()){
            index = next+1;
            next = find_next_node(s,index);
            cur = new TreeNode(Integer.parseInt(s.substring(index, next)));
            if(stack[head-1].left == null) stack[head-1].left = cur;
            else stack[head-1].right = cur;
            stack[head++] = cur;
            
            while(next < s.length() && s.charAt(next) == ')'){
                head--;
                next++;
            }
        }
        
        return stack[0];
    }
    
    
    
    public int find_next_node(String s, int index){
        while(index < s.length() && s.charAt(index) != '(' && s.charAt(index) != ')') index++;
        return index;
    }
    
}
```