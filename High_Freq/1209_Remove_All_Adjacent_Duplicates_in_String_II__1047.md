# 1209. Remove All Adjacent Duplicates in String II
You are given a string  `s`  and an integer  `k`, a  `k`  **duplicate removal**  consists of choosing  `k`  adjacent and equal letters from  `s`  and removing them, causing the left and the right side of the deleted substring to concatenate together.

We repeatedly make  `k`  **duplicate removals**  on  `s`  until we no longer can.

Return the final string after all such duplicate removals have been made. It is guaranteed that the answer is unique.

**Example 1:**

**Input:** s = "abcd", k = 2
**Output:** "abcd"
**Explanation:** There's nothing to delete.

**Example 2:**

**Input:** s = "deeedbbcccbdaa", k = 3
**Output:** "aa"
**Explanation:** First delete "eee" and "ccc", get "ddbbbdaa"
Then delete "bbb", get "dddaa"
Finally delete "ddd", get "aa"

**Example 3:**

**Input:** s = "pbbcggttciiippooaais", k = 2
**Output:** "ps"

**Constraints:**

-   `1 <= s.length <= 105`
-   `2 <= k <= 104`
-   `s`  only contains lower case English letters.

# Solution 1: Direct Mofidify from 1047 (Takes extremly long because of the inner loop)
```
class Solution {
    public String removeDuplicates(String s, int k) {
        if(s.length() < k) return s;
        char[] word = s.toCharArray();
        int index = k-2;
        
        for(int i = k-1; i < word.length; i++){
            if(index < k-2 || word[i] != word[index]) word[++index] = word[i];
            else{
                int cnt = 0;
                // because of this!!!!!
                while(index >= cnt && word[i] == word[index-cnt]) cnt++;  
                if(cnt < k - 1) word[++index] = word[i];
                else index -= cnt; 
            }
        }
        
        return (new String(word)).substring(0,index+1);
    }
}
```

# Solution 2: Using Another Array to Count Number of Same Chars (Beat 100%)
```
class Solution {
    public String removeDuplicates(String s, int k) {
        if(s.length() < k) return s;
        char[] word = s.toCharArray();
        int[] cnt = new int[word.length];
        int index = -1;
        
        for(int i = 0; i < word.length; i++){
            if(index < 0 || word[i] != word[index]){
                word[++index] = word[i];
                cnt[index] = 1;
            }else{
                cnt[index]++;
                if(cnt[index] == k) index--;
            }
        }
        
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i <= index; i++){
            while(cnt[i] > 0){
                sb.append(word[i]);
                cnt[i]--;
            }
        }
        
        return sb.toString();
    }
}
```

# Solution 3: Using Stack (Beat 43%)
```
class Solution {
    
    class Letter{
        int start;
        int cnt;
        char ch;
        Letter(){}
        Letter(int _start, int _cnt, char _c){
            this.start = _start;
            this.cnt = _cnt;
            this.ch = _c;
        }
    }
    
    public String removeDuplicates(String s, int k) {
        if(s.length() < k) return s;
        List<Letter> stack = new ArrayList<Letter>();
        stack.add(new Letter(0,1,s.charAt(0)));
        for(int i = 1; i < s.length(); i++){
            char ch = s.charAt(i);
            Letter let = new Letter();
            if(stack.size() > 0) let = stack.get(stack.size() - 1);
            
            if(let != null && ch == let.ch){
                let.cnt++;
                if(let.cnt == k) stack.remove(stack.size() - 1);
            }else stack.add(new Letter(i,1,ch));
        }
        
        StringBuilder sb = new StringBuilder();
        for(Letter l: stack) {
            for(;l.cnt > 0; l.cnt--) sb.append(l.ch+"");
        }
        return sb.toString();
    }
}
```