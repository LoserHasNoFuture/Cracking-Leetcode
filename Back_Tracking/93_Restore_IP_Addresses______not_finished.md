# 93. Restore IP Addresses
Given a string  `s`  containing only digits, return all possible valid IP addresses that can be obtained from  `s`. You can return them in  **any**  order.

A  **valid IP address**  consists of exactly four integers, each integer is between  `0`  and  `255`, separated by single dots and cannot have leading zeros. For example, "0.1.2.201" and "192.168.1.1" are  **valid**  IP addresses and "0.011.255.245", "192.168.1.312" and "192.168@1.1" are  **invalid**  IP addresses.

**Example 1:**

**Input:** s = "25525511135"
**Output:** ["255.255.11.135","255.255.111.35"]

**Example 2:**

**Input:** s = "0000"
**Output:** ["0.0.0.0"]

**Example 3:**

**Input:** s = "1111"
**Output:** ["1.1.1.1"]

**Example 4:**

**Input:** s = "010010"
**Output:** ["0.10.0.10","0.100.1.0"]

**Example 5:**

**Input:** s = "101023"
**Output:** ["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]

**Constraints:**

-   `0 <= s.length <= 3000`
-   `s`  consists of digits only.

# Solution 1: BackTracking (Beat 67%)
```
class Solution {
    ArrayList<String> res = new ArrayList<String>();
    int len;
    public List<String> restoreIpAddresses(String s) {
        len = s.length();
        if(len < 4  || len > 12) return res;
        char[] word = new char[len + 3];
        
        dfs(s,word,-1, 0);
        return res;
    }
    
    public void dfs(String s, char[] word, int index, int depth){
        if(index == len-1 && depth == 4) res.add(new String(word));
        if(depth >= 4 || index >= len - 1) return;
        
        int tmp = index;
        if(tmp >= 0) word[index + depth] = '.';
        
        int cnt = 0, num = 0;
        
        while(++index < len){
            num = num * 10 + Integer.parseInt(s.charAt(index) + "");
            word[index + depth] = s.charAt(index);
            cnt++;
            
            if(len - index - 1 > 3 * (3-depth) && num != 0) continue;
            
            if(num <= 255 && cnt <= 3){ 
                dfs(s, word, index, depth+1);
                if(s.length() - index - 1  == 3-depth || num == 0) break;
            }else break;
        }
        
    }
}
```