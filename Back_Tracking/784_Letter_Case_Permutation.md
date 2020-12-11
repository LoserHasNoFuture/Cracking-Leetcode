# 784. Letter Case Permutation
Given a string S, we can transform every letter individually to be lowercase or uppercase to create another string.

Return  _a list of all possible strings we could create_. You can return the output in  **any order**.

**Example 1:**

**Input:** S = "a1b2"
**Output:** ["a1b2","a1B2","A1b2","A1B2"]

**Example 2:**

**Input:** S = "3z4"
**Output:** ["3z4","3Z4"]

**Example 3:**

**Input:** S = "12345"
**Output:** ["12345"]

**Example 4:**

**Input:** S = "0"
**Output:** ["0"]

**Constraints:**

-   `S`  will be a string with length between  `1`  and  `12`.
-   `S`  will consist only of letters or digits.

# Solution 1: BackTracking 
Using char[], beat 100%.

Using StringBuilder, beat 56%.
```
class Solution {
    ArrayList<String> res = new ArrayList<String>();
    int n;
    boolean[] nums;
    
    public List<String> letterCasePermutation(String S) {
        n = S.length();
        nums = new boolean[n];
        char[] word = S.toCharArray();
        int first = -1;
        
        for(int i = 0; i < S.length(); i++) {
            char c = S.charAt(i);
            if(c >= '0' && c <= '9') nums[i] = true;
            else if(first == -1) first = i;
        }
        res.add(S);

        dfs(word, -1);

        return res;
    }
    
    
    public void dfs(char[] word, int index){
        
        char before = ' ';
        char after = ' ';
        if(index >= 0){
            before = word[index];
            if(before >= 'a' && before <= 'z') after = (char)(before - 32);
            else after = (char)(before + 32);
            
            word[index] = after;
            res.add(new String(word));
        }
        
        for(int i = index + 1; i < n; i++ ){
            if(!nums[i]) dfs(word, i);
        }

        if(index >= 0) word[index] = before;
    }
}
```