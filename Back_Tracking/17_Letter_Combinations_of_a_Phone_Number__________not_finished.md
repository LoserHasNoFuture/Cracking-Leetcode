# 17. Letter Combinations of a Phone Number
Given a string containing digits from  `2-9`  inclusive, return all possible letter combinations that the number could represent. Return the answer in  **any order**.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

**Example 1:**

**Input:** digits = "23"
**Output:** ["ad","ae","af","bd","be","bf","cd","ce","cf"]

**Example 2:**

**Input:** digits = ""
**Output:** []

**Example 3:**

**Input:** digits = "2"
**Output:** ["a","b","c"]

**Constraints:**

-   `0 <= digits.length <= 4`
-   `digits[i]`  is a digit in the range  `['2', '9']`.

# Solution 1: BackTracking (Beat 49.5%)
```
class Solution {
    ArrayList<String> res = new ArrayList<String>(); 
    char[][] letters = new char[10][4];
    public List<String> letterCombinations(String digits) {
        if(digits.length() == 0) return res;
        
        char a = 'a';
        for(int i = 2; i <= 9; i++){
            int cnt = (i == 9 || i == 7) ? 4:3;
            for(int j = 0; j < cnt; j++) letters[i][j] = (char)a++;
        }
        
        char[] word = new char[digits.length()];
        int[] nums = new int[digits.length()];
        for(int i = 0; i < digits.length(); i++) nums[i] = Integer.parseInt(digits.charAt(i)+"");
        
        dfs(nums, -1, word);
        
        return res;
    }
    
    public void dfs(int[] nums, int index, char[] word){
        if(index == nums.length - 1) {
            res.add(new String(word));
            return;
        }
        
        int num = nums[index+1];
        
        int cnt = (num == 9 || num == 7) ? 4: 3;
        for(int i = 0; i < cnt; i++){
            word[index+1] = letters[num][i];
            dfs(nums,index+1,word);
        }
        
    }
}
```