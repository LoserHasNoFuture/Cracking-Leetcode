# 428. Serialize and Deserialize N-ary Tree 536 606 449 297
Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize an N-ary tree. An N-ary tree is a rooted tree in which each node has no more than N children. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that an N-ary tree can be serialized to a string and this string can be deserialized to the original tree structure.

For example, you may serialize the following  `3-ary`  tree

![](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

as  `[1 [3[5 6] 2 4]]`. Note that this is just an example, you do not necessarily need to follow this format.

Or you can follow LeetCode's level order traversal serialization format, where each group of children is separated by the null value.

![](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)

For example, the above tree may be serialized as  `[1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]`.

You do not necessarily need to follow the above-suggested formats, there are many more different formats that work so please be creative and come up with different approaches yourself.

**Example 1:**

**Input:** root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
**Output:** [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]

**Example 2:**

**Input:** root = [1,null,3,2,4,null,5,6]
**Output:** [1,null,3,2,4,null,5,6]

**Example 3:**

**Input:** root = []
**Output:** []

**Constraints:**

-   The number of nodes in the tree is in the range  `[0, 104]`.
-   `0 <= Node.val <= 104`
-   The height of the n-ary tree is less than or equal to  `1000`
-   Do not use class member/global/static variables to store states. Your encode and decode algorithms should be stateless.

# Solution 1: PreOrder Traversal (Using splitter () and , )
```
class Codec {
    // Encodes a tree to a single string.
    public String serialize(Node root) {
        StringBuilder sb = new StringBuilder();
        dfs(root,sb,true);
        return sb.toString();
    }
    
    public void dfs(Node root, StringBuilder sb, boolean last){
        if(root == null) return;
        sb.append(root.val);
        
        if(root.children.size()>0){
            sb.append("(");
            for(int i = 0; i < root.children.size(); i++) {
                dfs(root.children.get(i),sb, i == root.children.size() - 1);
            }
            sb.append(")");
        }
        
        if(!last) sb.append(",");
    }
	
    // Decodes your encoded data to tree.
    public Node deserialize(String s) {
        if(s.length() == 0) return null;
        Node[] stack = new Node[10000];
        
        int index = 0, head = 0;
        int next = find_next_node(s,index);
        
        Node cur = new Node(Integer.parseInt(s.substring(index, next)),new ArrayList<Node>());
        stack[head++] = cur;
        
        Node root = cur;
        
        index = find_next_start(s, next+1);
        while(index < s.length()){
            next = find_next_node(s,index);
            cur = new Node(Integer.parseInt(s.substring(index, next)), new ArrayList<Node>());
            
            stack[head-1].children.add(cur);
            stack[head++] = cur;
            
            while(next < s.length() && (s.charAt(next) == ')' || s.charAt(next) == ',')){
                head--;
                next++;
            }
            
            index = find_next_start(s, next);
        }
        
        return stack[0];
    }
    
    public int find_next_start(String s, int index){
        while(index < s.length() && (s.charAt(index) == ',' 
              || s.charAt(index) == '(' || s.charAt(index) == ')')) index++;
        return index;
    }
    
    public int find_next_node(String s, int index){
        while(index < s.length() && s.charAt(index) != ',' 
              && s.charAt(index) != '(' && s.charAt(index) != ')') index++;
        return index;
    }
}
```

# Solution 2: Pre-order Traversal (record the number of children)
Refer from: [https://leetcode.com/problems/serialize-and-deserialize-n-ary-tree/discuss/151421/Java-preorder-recursive-solution-using-queue](https://leetcode.com/problems/serialize-and-deserialize-n-ary-tree/discuss/151421/Java-preorder-recursive-solution-using-queue)
```
class Codec {

    // Encodes a tree to a single string.
    public String serialize(Node root) {
        List<String> list=new LinkedList<>();
        serializeHelper(root,list);
        // System.out.println(String.join(",",list));
        return String.join(",",list);
    }
    
    private void serializeHelper(Node root, List<String> list){
        if(root==null){
            return;
        }else{
            list.add(String.valueOf(root.val));
            list.add(String.valueOf(root.children.size()));
            for(Node child:root.children){
                serializeHelper(child,list);
            }
        }
    }

    // Decodes your encoded data to tree.
    int index = 0;
    public Node deserialize(String data) {
        if(data.isEmpty())
            return null;
        
        String[] ss=data.split(",");
        index = 0;
        return deserializeHelper(ss);
    }
    
    private Node deserializeHelper(String[] str){
        Node root=new Node();
        root.val=Integer.parseInt(str[index++]);
        int size=Integer.parseInt(str[index++]);
        root.children=new ArrayList<Node>(size);
        for(int i=0;i<size;i++){
            root.children.add(deserializeHelper(str));
        }
        return root;
    }
}
```
