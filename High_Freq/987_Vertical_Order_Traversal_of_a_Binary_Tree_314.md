# 987. Vertical Order Traversal of a Binary Tree
Given the  `root`  of a binary tree, calculate the  **vertical order traversal**  of the binary tree.

For each node at position  `(row, col)`, its left and right children will be at positions  `(row + 1, col - 1)`  and  `(row + 1, col + 1)`  respectively. The root of the tree is at  `(0, 0)`.

The  **vertical order traversal**  of a binary tree is a list of top-to-bottom orderings for each column index starting from the leftmost column and ending on the rightmost column. There may be multiple nodes in the same row and same column. In such a case, sort these nodes by their values.

Return  _the  **vertical order traversal**  of the binary tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/29/vtree1.jpg)

**Input:** root = [3,9,20,null,null,15,7]
**Output:** [[9],[3,15],[20],[7]]
**Explanation:**
Column -1: Only node 9 is in this column.
Column 0: Nodes 3 and 15 are in this column in that order from top to bottom.
Column 1: Only node 20 is in this column.
Column 2: Only node 7 is in this column.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/29/vtree2.jpg)

**Input:** root = [1,2,3,4,5,6,7]
**Output:** [[4],[2],[1,5,6],[3],[7]]
**Explanation:**
Column -2: Only node 4 is in this column.
Column -1: Only node 2 is in this column.
Column 0: Nodes 1, 5, and 6 are in this column.
          1 is at the top, so it comes first.
          5 and 6 are at the same position (2, 0), so we order them by their value, 5 before 6.
Column 1: Only node 3 is in this column.
Column 2: Only node 7 is in this column.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/01/29/vtree3.jpg)

**Input:** root = [1,2,3,4,6,5,7]
**Output:** [[4],[2],[1,5,6],[3],[7]]
**Explanation:**
This case is the exact same as example 2, but with nodes 5 and 6 swapped.
Note that the solution remains the same since 5 and 6 are in the same location and should be ordered by their values.

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 1000]`.
-   `0 <= Node.val <= 1000`

# Solution 1: DFS
Use priority queue
```
class Solution {
    
    class Node {
        int val;
        int x;
        int y;
        Node(int Val, int X, int Y){
            val = Val;
            x = X;
            y = Y;
        }
    }
    
    public List<List<Integer>> verticalTraversal(TreeNode root) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if(root == null) return res;
        PriorityQueue<Node> queue = new PriorityQueue<Node>(new Comparator<Node>(){
            public int compare(Node node1, Node node2){
                if(node1.x > node2.x) return 1;
                if(node1.x < node2.x) return -1;
                if(node1.y > node2.y) return -1;
                if(node1.y < node2.y) return 1;
                return node1.val - node2.val;
            }
        });
        dfs(root,0,0,queue);
        ArrayList<Integer> arr = new ArrayList<Integer>();
        Node prev = null;
        while(!queue.isEmpty()){
            Node cur = queue.poll();
            if(prev != null && cur.x != prev.x){
                res.add(arr);
                arr = new ArrayList<Integer>();
            }
            arr.add(cur.val);
            prev = cur;
        }
        res.add(arr);
        return res;
    }
    
    public void dfs(TreeNode root, int x, int y, PriorityQueue<Node> queue){
        if(root == null) return;
        queue.offer(new Node(root.val,x,y));
        dfs(root.left,x-1,y-1,queue);
        dfs(root.right,x+1,y-1,queue);
    }
    

}
```

# Solution 2: BFS
```
class Solution {

    class Pair{
        TreeNode node;
        int x;  //horizontal
        int y;  //depth
        Pair(TreeNode n, int x, int y)
        {
            node = n;
            this.x = x;
            this.y = y;
        }
    }
    
    public List<List<Integer>> verticalTraversal(TreeNode root) {
        List<List<Integer>> lists = new ArrayList<>();
        Map<Integer, List<Pair>> map = new HashMap<>(); //x -> list (some nodes might have same y in the list)
        
        Queue<Pair> q = new LinkedList<>();
        q.add(new Pair(root, 0, 0));
        int min = 0, max = 0;
        while(!q.isEmpty())
        {
            Pair p = q.remove(); 
            
            min = Math.min(min, p.x);
            max = Math.max(max, p.x);
            
            if(!map.containsKey(p.x)) 
                map.put(p.x, new ArrayList<>());
            map.get(p.x).add(p);
            
            if(p.node.left!=null) q.add(new Pair(p.node.left, p.x-1, p.y+1));
            if(p.node.right!=null) q.add(new Pair(p.node.right, p.x+1, p.y+1));
        }        

        for(int i=min; i<=max; i++)
        {
            Collections.sort(map.get(i), new Comparator<Pair>(){
                public int compare(Pair a, Pair b)
                {
                    if(a.y==b.y) //when y is equal, sort it by value
                        return a.node.val - b.node.val;
                    return 0; //otherwise don't change the order as BFS ganrantees that top nodes are visited first
                }
            });
            List<Integer> list = new ArrayList<>();
            for(int j=0; j<map.get(i).size(); j++)
            {
                list.add(map.get(i).get(j).node.val);
            }
            lists.add(list);
        }
        return lists;   
    }
}
```