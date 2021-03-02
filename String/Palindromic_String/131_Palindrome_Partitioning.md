# 131. Palindrome Partitioning
Given a string  `s`, partition  `s`  such that every substring of the partition is a  **palindrome**. Return all possible palindrome partitioning of  `s`.

A  **palindrome**  string is a string that reads the same backward as forward.

**Example 1:**

**Input:** s = "aab"
**Output:** [["a","a","b"],["aa","b"]]

**Example 2:**

**Input:** s = "a"
**Output:** [["a"]]

**Constraints:**

-   `1 <= s.length <= 16`
-   `s`  contains only lowercase English letters.

# Solution 1: Back Tracking (Beat 70%)
```
class Solution {
    List<List<String>> res = new ArrayList<List<String>>();
    char[] word;
    public List<List<String>> partition(String s) {
        word = s.toCharArray();
        
        ArrayList<String> arr = new ArrayList<String>();
        
        dfs(0, s, arr);
        
        return res;
    }
    
    public void dfs(int index, String s, ArrayList<String> arr){
        if(index == word.length) res.add(new ArrayList<String>(arr));
        
        for(int i = index; i < word.length; i++){
            if(!isPanlindrom(index, i)) continue;
            arr.add(s.substring(index,i+1));
            dfs(i+1,s,arr);
            arr.remove(arr.size()-1);
        }
    }
    
    public boolean isPanlindrom(int left, int right){
        while(left <= right){
            if(word[left] != word[right]) return false;
            left++;right--;
        }
        return true;
    }
}
```

# Solution 2: Malancher's Algo (Not good for this one, Beat 56%)
```
class Solution {
    List<List<String>> res = new ArrayList<List<String>>();
    char[] word;
    public List<List<String>> partition(String s) {
        if(s.length() == 0) return res;
        int[] dp = new int[s.length()*2 + 1];
        int[] palin = new int[s.length()];
        word = s.toCharArray();
        malancher_algo(s,dp, palin);
        
        List<String> cur = new ArrayList<String>();
        dfs(s, palin, 0, cur);    
        
        return res;
    }
    
    public void dfs(String s, int[] palin, int index, List<String> cur){
        if(index == s.length()) {
            res.add(new ArrayList<String>(cur));
            return;
        }
        
        for(int i = index + 1; i <= palin[index] + 1; i++){
            if(!isPanlindrom(index, i-1)) continue;
            cur.add(s.substring(index, i));
            dfs(s, palin, i,cur);
            cur.remove(cur.size() - 1);
        }
    }

    public void malancher_algo(String s, int[] dp, int[] palin){        
        int max = 0;
        int max_center = 0;
        
        for(int i = 1; i < dp.length; i++){
            if(i < max){
                int mirror = max_center * 2 - i;
                if(dp[mirror] <  max - i) dp[i] = dp[mirror];
                else if(dp[mirror] >  max - i) dp[i] = max - i;
                else{
                    int k = max - i;
                    while(k <= i && k + i < dp.length){
                        if(((k+i)&1) == 0 || word[(i+k)/2] == word[(i-k)/2]) k++;
                        else break;
                    }
                    k--;
                    dp[i] = k;
                }
                
            }else{
                int k = 0;
                while(k <= i && k + i < dp.length){
                    if(((k+i)&1) == 0 || word[(i+k)/2] == word[(i-k)/2]) k++;
                    else break;
                }
                k--;
                dp[i] = k; 
            }
            
            if(dp[i] + i > max){
                max = dp[i] + i;
                max_center = i;
            }
               
            for(int start = (i - dp[i] + 1) >>> 1; start <= i/2 && start < word.length; start++){
                int end;
                if(dp[i] == 0) end = start;    
                else end = i - start - 1;
                
                if(palin[start] < end) palin[start] = end;
            }
            
        }
        
    }
    
    
    public boolean isPanlindrom(int left, int right){
        while(left <= right){
            if(word[left] != word[right]) return false;
            left++;right--;
        }
        return true;
    } 
}
```