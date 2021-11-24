# 449. Serialize and Deserialize BST
Serialization is converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a  **binary search tree**. There is no restriction on how your serialization/deserialization algorithm should work. You need to ensure that a binary search tree can be serialized to a string, and this string can be deserialized to the original tree structure.

**The encoded string should be as compact as possible.**

**Example 1:**

**Input:** root = [2,1,3]
**Output:** [2,1,3]

**Example 2:**

**Input:** root = []
**Output:** []

**Constraints:**

-   The number of nodes in the tree is in the range  `[0, 104]`.
-   `0 <= Node.val <= 104`
-   The input tree is  **guaranteed**  to be a binary search tree.

# Solution
```
public class Codec {

    // Encodes a tree to a single string.
    StringBuilder sb;
    public String serialize(TreeNode root) {
        sb = new StringBuilder();
        dfs(root);
        return sb.toString();
    }
    
    public void dfs(TreeNode root){
        if(root == null)
            return;
        
        sb.append(root.val);
        // It is crucial to check whether they are null
        if(root.left != null) {
            sb.append(",");
            dfs(root.left);
        }
        
        if(root.right != null) {
            sb.append(",");
            dfs(root.right);
        }
    }

    // Decodes your encoded data to tree.
    int index;
    public TreeNode deserialize(String data) {
        if(data.length() == 0) return null;
        index = 0;
        String[] arr = data.split(",");
        int[] preorder = new int[arr.length];
        for(int i = 0; i < arr.length; i++){
            preorder[i] = Integer.valueOf(arr[i]);
        }
        return dfs(preorder, Integer.MIN_VALUE, Integer.MAX_VALUE);
    }
    
    public TreeNode dfs(int[] preorder, int lowerbound, int upperbound){
        if(index >= preorder.length || preorder[index] < lowerbound || preorder[index] > upperbound) return null;
        TreeNode root = new TreeNode(preorder[index++]);
        root.left = dfs(preorder, lowerbound, root.val);
        root.right = dfs(preorder, root.val, upperbound);
        return root;
    }
}
```