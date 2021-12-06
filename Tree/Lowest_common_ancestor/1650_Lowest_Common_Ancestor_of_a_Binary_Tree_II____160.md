# 1650. Lowest Common Ancestor of a Binary Tree III
Given two nodes of a binary tree  `p`  and  `q`, return  _their lowest common ancestor (LCA)_.

Each node will have a reference to its parent node. The definition for  `Node`  is below:

class Node {
    public int val;
    public Node left;
    public Node right;
    public Node parent;
}

According to the  **[definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor)**: "The lowest common ancestor of two nodes p and q in a tree T is the lowest node that has both p and q as descendants (where we allow  **a node to be a descendant of itself**)."

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Input:** root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
**Output:** 3
**Explanation:** The LCA of nodes 5 and 1 is 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Input:** root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
**Output:** 5
**Explanation:** The LCA of nodes 5 and 4 is 5 since a node can be a descendant of itself according to the LCA definition.

**Example 3:**

**Input:** root = [1,2], p = 1, q = 2
**Output:** 1

**Constraints:**

-   The number of nodes in the tree is in the range  `[2, 105]`.
-   `-109  <= Node.val <= 109`
-   All  `Node.val`  are  **unique**.
-   `p != q`
-   `p`  and  `q`  exist in the tree.


# Solution 1: HashSet (50%)
```
class Solution {
    
    public Node lowestCommonAncestor(Node p, Node q) {
        HashSet<Node> set = new HashSet<Node>();
        
        while(p != null){
            set.add(p);
            p = p.parent;
        }
        
        while(q != null){
            if(set.contains(q)) return q;
            q = q.parent;
        }
        
        return null;
    }
}
```

# Solution 2: Intersection of Linked List (Beat 100%)
```
class Solution {
    
    public Node lowestCommonAncestor(Node p, Node q) {
        if(p == null || q == null) return null;
        
        int step1 = len_to_root(p);
        int step2 = len_to_root(q);
        
        if(step1 > step2) p = move_steps(step1-step2,p);
        else q = move_steps(-step1+step2,q);
        
        while (p != q){
            p = p.parent;
            q = q.parent;
        }
        
        return p;
    }
    
    public Node move_steps(int k, Node p){
        while(k-- > 0){
            p = p.parent;
        }
        return p;
    }
    
    public int len_to_root(Node p){
        int step = 0;
        
        while(p != null){
            p = p.parent;
            step++;
        }
        
        return step;
    }
}
```



```
class Solution {
    
    public Node lowestCommonAncestor(Node p, Node q) {
        if(p == null || q == null) return null;
        
        Node a = p;
        Node b = q;
        
        while (a != b){
            a = a == null? q: a.parent;
            b = b == null? p: b.parent;
        }
        
        return a;
    }
}
```
