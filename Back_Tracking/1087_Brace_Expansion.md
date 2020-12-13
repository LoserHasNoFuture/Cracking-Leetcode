# 1087. Brace Expansion
A string  `S` represents a list of words.

Each letter in the word has 1 or more options. If there is one option, the letter is represented as is. If there is more than one option, then curly braces delimit the options. For example,  `"{a,b,c}"`  represents options  `["a", "b", "c"]`.

For example,  `"{a,b,c}d{e,f}"`  represents the list  `["ade", "adf", "bde", "bdf", "cde", "cdf"]`.

Return all words that can be formed in this manner, in lexicographical order.

**Example 1:**

**Input:** "{a,b}c{d,e}f"
**Output:** ["acdf","acef","bcdf","bcef"]

**Example 2:**

**Input:** "abcd"
**Output:** ["abcd"]

**Note:**

1.  `1 <= S.length <= 50`
2.  There are no nested curly brackets.
3.  All characters inside a pair of consecutive opening and ending curly brackets are different.

# Solution 1: BackTracking (Beat 95%)
```
class Solution {
    
    ArrayList<String> res = new ArrayList<String>();
    public String[] expand(String S) {
        char[] word = S.toCharArray();
        StringBuilder sb = new StringBuilder();
        
        dfs(word,sb,-1);
        
        String[] ret = new String[res.size()];
        for(int i = 0; i < res.size(); i++)
            ret[i] = res.get(i);
        
        Arrays.sort(ret);
        return ret;
    }
    
    public void dfs(char[] word, StringBuilder sb, int index){

        int i = index + 1;
        for(; i < word.length; i++){
            if(word[i] == '{') break;
            sb.append(word[i]);
        }
        
        if(i == word.length) res.add(sb.toString());
        else{
            i++;
            int end = i+1;
            for(; end < word.length; end++){
                if(word[end] == '}') break;
            }
            
            int cur_len = sb.length();
            for(int next = i; next < end; next++){
                while(next < end && word[next] != ','){
                    sb.append(word[next]);
                    next++;
                }
                
                dfs(word,sb,end);
                sb.delete(cur_len,sb.length());
            }
        }
        
    }
}
```