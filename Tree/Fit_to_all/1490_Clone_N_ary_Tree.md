# 1490. Clone N-ary Tree
Given a  `root` of an N-ary tree, return a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) (clone) of the tree.

Each node in the n-ary tree contains a val (`int`) and a list (`List[Node]`) of its children.

class Node {
    public int val;
    public List<Node> children;
}

_Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples)._

**Follow up:** Can your solution work for the  [graph problem](https://leetcode.com/problems/clone-graph/)?

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

**Input:** root = [1,null,3,2,4,null,5,6]
**Output:** [1,null,3,2,4,null,5,6]

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)

**Input:** root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
**Output:** [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]

**Constraints:**

-   The depth of the n-ary tree is less than or equal to  `1000`.
-   The total number of nodes is between  `[0, 10^4]`.

# Solution 1: DFS
Recursion:
```
class Solution {
    public Node cloneTree(Node root) {
        if(root == null) return root;
        Node new_root = new Node(root.val);
        
        for(Node child : root.children){
            new_root.children.add(cloneTree(child));
        }
        
        return new_root;
    }
}
```

Iteration:
```
class Solution {
    public Node cloneTree(Node root) {
        if(root == null) return root;
        Node new_root = new Node(root.val);
        
        Deque<Node> stack = new ArrayDeque<Node>();
        stack.push(new_root);
        stack.push(root);
        
        
        while(!stack.isEmpty()){
            Node cur = stack.pop();
            Node new_cur = stack.pop();
            
            for(Node child: cur.children){
                Node new_child = new Node(child.val);
                new_cur.children.add(new_child);
                stack.push(new_child);
                stack.push(child);
            }
        }
        
        return new_root;
    }
}
```

# Solution 2: BFS
```
class Solution {
    public Node cloneTree(Node root) {
        if(root == null) return root;
        Node new_root = new Node(root.val);
        
        Queue<Node> q = new LinkedList<Node>();
        q.offer(root);
        q.offer(new_root);
        
        while(!q.isEmpty()){
            Node cur = q.poll();
            Node new_cur = q.poll();
            
            for(Node child: cur.children){
                Node new_child = new Node(child.val);
                new_cur.children.add(new_child);
                q.offer(child);
                q.offer(new_child);
            }
        }
       
        return new_root;
    }
}
```