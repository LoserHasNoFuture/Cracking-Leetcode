# 1177. Can Make Palindrome from Substring
Given a string  `s`, we make queries on substrings of  `s`.

For each query  `queries[i] = [left, right, k]`, we may  **rearrange** the substring  `s[left], ..., s[right]`, and then choose  **up to**  `k`  of them to replace with any lowercase English letter.

If the substring is possible to be a palindrome string after the operations above, the result of the query is  `true`. Otherwise, the result is  `false`.

Return an array  `answer[]`, where  `answer[i]`  is the result of the  `i`-th query  `queries[i]`.

Note that: Each letter is counted  **individually**  for replacement so if for example `s[left..right] = "aaa"`, and  `k = 2`, we can only replace two of the letters. (Also, note that the initial string  `s` is never modified by any query.)

**Example :**

**Input:** s = "abcda", queries = [[3,3,0],[1,2,0],[0,3,1],[0,3,2],[0,4,1]]
**Output:** [true,false,false,true,true]
**Explanation:**
queries[0] : substring = "d", is palidrome.
queries[1] : substring = "bc", is not palidrome.
queries[2] : substring = "abcd", is not palidrome after replacing only 1 character.
queries[3] : substring = "abcd", could be changed to "abba" which is palidrome. Also this can be changed to "baab" first rearrange it "bacd" then replace "cd" with "ab".
queries[4] : substring = "abcda", could be changed to "abcba" which is palidrome.

**Constraints:**

-   `1 <= s.length, queries.length <= 10^5`
-   `0 <= queries[i][0] <= queries[i][1] < s.length`
-   `0 <= queries[i][2] <= s.length`
-   `s`  only contains lowercase English letters.

# Solution 1: Count number of odds (Beat 70%)
```
class Solution {
    public List<Boolean> canMakePaliQueries(String s, int[][] queries) {
        int[][] dp = new int[s.length() + 1][26];
        
        for(int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            for(int j = 0; j < 26; j++){
                if(c - 97 == j) dp[i+1][j] = dp[i][j] + 1;
                else dp[i+1][j] = dp[i][j];
            }
        }
        
        List<Boolean> res = new ArrayList<Boolean>();     
        for(int[] query : queries){

            int num_of_odds = 0;
            for(int j = 0; j < 26; j++){
                num_of_odds += (dp[query[1]+1][j] - dp[query[0]][j])%2;
            }
            if(num_of_odds - query[2]*2 <= 1) res.add(true);
            else res.add(false);
        }
        
        return res;
    }
}
```

# Solution 2: Similar idea, But Using each bit to represent each character (Beat 100%)
```
class Solution {
    public List canMakePaliQueries(String s, int[][] queries) {

        int str_len = s.length();
        int j = 0;
        List<Boolean> results = new ArrayList<>();
        int[] masks = new int[str_len + 1];

        int mask = 0;
        for(int i = 0 ; i < str_len; i++){
            mask ^= (1 << (s.charAt(i) - 'a'));
            masks[++j] = mask;
        }

        for(int [] query : queries){
            if(query[2] >= 13){
                results.add(true);
            }else{
                results.add(Integer.bitCount(masks[query[1] + 1] ^ masks[query[0]]) /2 <= query[2]);
            }
        }

        return results;
    }
}
```