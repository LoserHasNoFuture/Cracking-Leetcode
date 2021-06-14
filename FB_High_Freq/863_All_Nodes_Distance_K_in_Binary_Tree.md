# 863. All Nodes Distance K in Binary Tree
We are given a binary tree (with root node `root`), a  `target`  node, and an integer value  `k`.

Return a list of the values of all nodes that have a distance  `k`  from the  `target`  node. The answer can be returned in any order.

**Example 1:**

**Input:** root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, k = 2

**Output:** [7,4,1]

**Explanation:** 
The nodes that are a distance 2 from the target node (with value 5)
have values 7, 4, and 1.

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/06/28/sketch0.png)

Note that the inputs "root" and "target" are actually TreeNodes.
The descriptions of the inputs above are just serializations of these objects.

**Note:**

1.  The given tree is non-empty.
2.  Each node in the tree has unique values `0 <= node.val <= 500`.
3.  The  `target` node is a node in the tree.
4.  `0 <= k <= 1000`.

# Solution 1: Using HashMap (Beat 87%)  O(n)
```
class Solution {
    
    Map<TreeNode, Integer> map = new HashMap<>();
        
    public List<Integer> distanceK(TreeNode root, TreeNode target, int K) {
        List<Integer> res = new LinkedList<>();
        find(root, target);
        dfs(root, target, K, map.get(root), res);
        return res;
    }
    
    // find target node first and store the distance in that path that we could use it later directly
    private int find(TreeNode root, TreeNode target) {
        if (root == null) return -1;
        if (root == target) {
            map.put(root, 0);
            return 0;
        }
        int left = find(root.left, target);
        if (left >= 0) {
            map.put(root, left + 1);
            return left + 1;
        }
		int right = find(root.right, target);
		if (right >= 0) {
            map.put(root, right + 1);
            return right + 1;
        }
        return -1;
    }
    
    private void dfs(TreeNode root, TreeNode target, int K, int length, List<Integer> res) {
        if (root == null) return;
        if (map.containsKey(root)) length = map.get(root);
        if (length == K) res.add(root.val);
        dfs(root.left, target, K, length + 1, res);
        dfs(root.right, target, K, length + 1, res);
    }
}
```

# Solution 2: HashSet + DFS (Beat 97%)  O(n)
```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    HashSet<TreeNode> visited = new HashSet<TreeNode>();
    List<Integer> res = new ArrayList<Integer>();
    public List<Integer> distanceK(TreeNode root, TreeNode target, int k) {
        dfs(root,target,k,-1);
        return res;
    }
    
    public int dfs(TreeNode root, TreeNode target, int k, int dis){
        if(root == null || visited.contains(root) || dis > k) return dis;
        if(root == target) dis = 0;
        
        if(dis >= 0) visited.add(root);
        if(dis == k) res.add(root.val);    
        
        if(dis >= 0){
            dfs(root.left,target,k,dis+1);
            dfs(root.right,target,k,dis+1);    
        }else{
            int l = dfs(root.left,target,k,dis);
            if(l >= 0) {
                visited.add(root);
                if(l + 1 == k) res.add(root.val);
                dfs(root.right,target,k,l+2);
                dis = l + 1;
            }else{
                int r = dfs(root.right,target,k,dis);
                if(r >= 0){
                    visited.add(root);
                    if(r + 1 == k) res.add(root.val);
                    dfs(root.left,target,k,r+2);
                    dis = r + 1;
                }
            }
        }
        
        
        return dis;
    }
}
```