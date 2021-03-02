# 125. Valid Palindrome
Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

**Note:** For the purpose of this problem, we define empty string as valid palindrome.

**Example 1:**

**Input:** "A man, a plan, a canal: Panama"
**Output:** true

**Example 2:**

**Input:** "race a car"
**Output:** false

**Constraints:**

-   `s`  consists only of printable ASCII characters.

# Solution: Two Pointers
```
class Solution {
    static int OTHER = 0;
    static int UPPER_CASE = 1;
    static int LOWER_CASE = 2;
    static int NUMBER = 3;
    
    public boolean isPalindrome(String s) {
        if(s.length() <= 1) return true;
        
        int len = s.length();
        int end = s.length()-1;
        int start = 0;
        
        while(start < end){
            while(start < len && character_type(s.charAt(start)) == OTHER) start++;
            while(end >= 0 && character_type(s.charAt(end)) == OTHER) end--;
            System.out.println(start);
            System.out.println(end);
            if(start < end){
                char A = s.charAt(start);
                char B = s.charAt(end);
                if(character_type(A) == UPPER_CASE) A = (char) ((int)A+32);
                if(character_type(B) == UPPER_CASE) B = (char) ((int)B+32);
                if(A != B) return false;
                start++;
                end--;
            }
        }
        
        return true;
    }
    
    public int character_type(char c){
        if(c >= 'a' && c <= 'z') return LOWER_CASE;
        if(c >= 'A' && c <= 'Z') return UPPER_CASE;
        if(c >= '0' && c <= '9') return NUMBER;
        return OTHER;
    }
}
```

# Solution: Use Regular Expression And String API
Refer from: [https://leetcode.com/problems/valid-palindrome/discuss/39981/My-three-line-java-solution](https://leetcode.com/problems/valid-palindrome/discuss/39981/My-three-line-java-solution)
```
class Solution {
    public boolean isPalindrome(String s) {
        String actual = s.replaceAll("[^A-Za-z0-9]", "").toLowerCase();
        String rev = new StringBuffer(actual).reverse().toString();
        return actual.equals(rev);
    }
}
```

# Solution: Build a Dictionary

Refer from: [https://leetcode.com/problems/valid-palindrome/discuss/39993/3ms-java-solution(beat-100-of-java-solution)](https://leetcode.com/problems/valid-palindrome/discuss/39993/3ms-java-solution(beat-100-of-java-solution))
```
class Solution {
    private static final char[]charMap = new char[256];
    static{
        for(int i=0;i<10;i++){
            charMap[i+'0'] = (char)(1+i);  // numeric
        }
        for(int i=0;i<26;i++){
            charMap[i+'a'] = charMap[i+'A'] = (char)(11+i);  //alphabetic, ignore cases
        }
    }
    public boolean isPalindrome(String s) {
        char[]pChars = s.toCharArray();
        int start = 0,end=pChars.length-1;
        char cS,cE;
        while(start<end){
            cS = charMap[pChars[start]];
            cE = charMap[pChars[end]];
            if(cS!=0 && cE!=0){
                if(cS!=cE)return false;
                start++;
                end--;
            }else{
                if(cS==0)start++;
                if(cE==0)end--;
            }
        }
        return true;
    }
}
```