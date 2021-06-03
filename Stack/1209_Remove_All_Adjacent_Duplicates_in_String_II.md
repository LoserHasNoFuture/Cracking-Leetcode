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

# Solution 1: Using StringBuilder to mock Stack (Beat 43%)
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


# Solution 2: Using Array to Realize Stack (Beat 100%)
Refer from: [https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string-ii/discuss/392933/JavaC%2B%2BPython-Two-Pointers-and-Stack-Solution](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string-ii/discuss/392933/JavaC%2B%2BPython-Two-Pointers-and-Stack-Solution)
```
class Solution {
    
     public String removeDuplicates(String s, int k) {
        int i = 0, n = s.length(), count[] = new int[n];
        char[] stack = s.toCharArray();
        for (int j = 0; j < n; ++j, ++i) {
            stack[i] = stack[j];
            count[i] = i > 0 && stack[i - 1] == stack[j] ? count[i - 1] + 1 : 1;
            if (count[i] == k) i -= k;
        }
        return new String(stack, 0, i);
    }
}
```