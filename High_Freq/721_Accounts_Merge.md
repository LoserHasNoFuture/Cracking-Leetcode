# 721. Accounts Merge
Given a list of  `accounts`  where each element  `accounts[i]`  is a list of strings, where the first element  `accounts[i][0]`  is a name, and the rest of the elements are  **emails**  representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some common email to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails  **in sorted order**. The accounts themselves can be returned in  **any order**.

**Example 1:**

**Input:** accounts = [["John","johnsmith@mail.com","john_newyork@mail.com"],["John","johnsmith@mail.com","john00@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
**Output:** [["John","john00@mail.com","john_newyork@mail.com","johnsmith@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
**Explanation:**
The first and third John's are the same person as they have the common email "johnsmith@mail.com".
The second John and Mary are different people as none of their email addresses are used by other accounts.
We could return these lists in any order, for example the answer [['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'], 
['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']] would still be accepted.

**Example 2:**

**Input:** accounts = [["Gabe","Gabe0@m.co","Gabe3@m.co","Gabe1@m.co"],["Kevin","Kevin3@m.co","Kevin5@m.co","Kevin0@m.co"],["Ethan","Ethan5@m.co","Ethan4@m.co","Ethan0@m.co"],["Hanzo","Hanzo3@m.co","Hanzo1@m.co","Hanzo0@m.co"],["Fern","Fern5@m.co","Fern1@m.co","Fern0@m.co"]]
**Output:** [["Ethan","Ethan0@m.co","Ethan4@m.co","Ethan5@m.co"],["Gabe","Gabe0@m.co","Gabe1@m.co","Gabe3@m.co"],["Hanzo","Hanzo0@m.co","Hanzo1@m.co","Hanzo3@m.co"],["Kevin","Kevin0@m.co","Kevin3@m.co","Kevin5@m.co"],["Fern","Fern0@m.co","Fern1@m.co","Fern5@m.co"]]

**Constraints:**

-   `1 <= accounts.length <= 1000`
-   `2 <= accounts[i].length <= 10`
-   `1 <= accounts[i][j] <= 30`
-   `accounts[i][0]`  consists of English letters.
-   `accounts[i][j] (for j > 0)`  is a valid email.


# Solution: Union Find
```
class Solution {
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        int n = accounts.size();
        int[] id = new int[n], sz = new int[n];
        for(int i = 0; i < n; i++) id[i] = i;
        // key: email, value: id
        HashMap<String, Integer> map = new HashMap<String, Integer>();
        
        link_accounts(accounts,n,id,sz,map);
        
        // key: id, value: list of emails
        HashMap<Integer, List<String>> tmp = new HashMap<Integer, List<String>>();
        
        parseMap(map,id,accounts,tmp);
        
        List<List<String>> res = new ArrayList<List<String>>();
        
        parseResults(tmp,res,accounts);
        
        return res;
    }
    
    public void parseResults(HashMap<Integer, List<String>> tmp, List<List<String>> res,
                            List<List<String>> accounts){
        for(Integer index: tmp.keySet()){
            String name = accounts.get(index).get(0);
            List<String> arr = tmp.get(index);
            
            Collections.sort(arr);
            arr.add(0,name);
            res.add(arr);
        }
    }
    
    public void parseMap(HashMap<String, Integer> map, int[] id, 
                         List<List<String>> accounts, HashMap<Integer, List<String>> tmp){
        for(String eml: map.keySet()){
            int root = find_root(id,map.get(eml));
            
            if(tmp.containsKey(root)){
                List<String> set = tmp.get(root);
                set.add(eml);
            }else{
                List<String> set = new ArrayList<String>();
                set.add(eml);
                tmp.put(root, set);
            }
        }
    }
    
    public void link_accounts(List<List<String>> accounts, int n, int[] id, int[] sz, HashMap<String, Integer> map){
        for(int i = 0; i < n; i++){
            List<String> acc = accounts.get(i);
            for(int j = 1; j < acc.size(); j++){
                String eml = acc.get(j);
                if(map.containsKey(eml)){
                    // union
                    int root1 = find_root(id,i);
                    int root2 = find_root(id,map.get(eml));
                    if(root1 == root2) continue;
                    if(sz[root1] > sz[root2]){
                        id[root2] = root1;
                        map.put(eml,root1);
                    }else{
                        id[root1] = root2;
                        sz[root2] = Math.max(sz[root1]+1,sz[root2]);
                        map.put(eml,root2);
                    }
                }else map.put(eml,i);
            }
        }
        
    }
    
    
    public int find_root(int[] id, int p){
        while(id[p] != p){
            id[p] = id[id[p]];
            p = id[p];
        }
        return p;
    }
}
```