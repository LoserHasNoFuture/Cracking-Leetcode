# 5. Longest Palindromic Substring

Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.

**Example 1:**

**Input:** "babad"
**Output:** "bab"
**Note:** "aba" is also a valid answer.

**Example 2:**

**Input:** "cbbd"
**Output:** "bb"

# Solution 1: Two Iterative Methods
```
public class Solution {
    
    String result = "";
    public String longestPalindrome(String s) {
        int len = s.length();
        if(len <= 1) return s;
        
        for(int i = 0; i < len-1; i++){
            check_palindromic(i,i,s);
            check_palindromic(i,i+1,s);
        }
        
        return result;
    }
    
    public void check_palindromic(int left, int right, String s){
        while(left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)){
            left--;
            right++;
        }
        if(right - left - 1 > result.length())
            result = s.substring(left+1,right);
        
    }

}
```

Add constriants on for-loop:
```
for(int i = 0; i < len-1 && i*2 >= result.length() && (len-i)*2 >= result.length(); i++){
    check_palindromic(i,i,s);
    check_palindromic(i,i+1,s);
}
```

Another iterative approach

Refer to: [https://leetcode.com/problems/longest-palindromic-substring/discuss/3060/(AC)-relatively-short-and-very-clear-Java-solution](https://leetcode.com/problems/longest-palindromic-substring/discuss/3060/(AC)-relatively-short-and-very-clear-Java-solution)
```
Example: "xxxbcbxxxxxa", (x is random character, not all x are equal) now we 
          are dealing with the last character 'a'. The current longest palindrome
          is "bcb" with length 3.
1. check "xxxxa" so if it is palindrome we could get a new palindrome of length 5.
2. check "xxxa" so if it is palindrome we could get a new palindrome of length 4.
3. do NOT check "xxa" or any shorter string since the length of the new string is 
   no bigger than current longest length.
4. do NOT check "xxxxxa" or any longer string because if "xxxxxa" is palindrome 
   then "xxxx" got  from cutting off the head and tail is also palindrom. It has 
   length > 3 which is impossible.'
```

```
class Solution {
    
    public String longestPalindrome(String s) {
        String res = "";
        if(s == null || s.length() == 0) return res;
        
        int curMax = 0;
        int index = 0;
        for(int i = 0; i < s.length(); i++){            
            if(isPalindrome(i,i-curMax - 1,s)){
                curMax += 2;
                index = i;
            } else if(isPalindrome(i,i-curMax,s)){ //"aaaaaa"
                curMax +=1;
                index = i;
            }
        }
        
        res = s.substring(index-curMax + 1, index+1);
        return res;
    }
    
    public boolean isPalindrome(int end, int begin, String s){
        if(begin<0) return false;
        while(begin<end){
        	if(s.charAt(begin++)!=s.charAt(end--)) return false;
        }
        return true;
    }
}
```

# Solution 2: Manacher's Algorithm (Beat 98%)
### Initial Idea 
Time Complexity O($n^2$), Space O($n$) 
```
class Solution {
    
// with no optimization on building helper
    public String longestPalindrome(String s) {
        if(s.length() <= 1) return s;
        int len = s.length();
        int center = 0;
        int max_length = 0;
        // insert an imaginary delimiter to string, ex. #b#a#b#a#d#
        // to make palindromic string to be always centrosymmetry
        int[] helper = new int[2*len+1];
        
        for(int i = 1; i < helper.length; i++){
            int count = 0;
            for(int j = 1; i-j>=0 && i+j < helper.length; j++){
            // imaginary delimiters are the same
                if((i-j)%2 == 0){
                    count++;
                    continue;
                }
                char A = s.charAt((i-j-1)/2);
                char B = s.charAt((i+j-1)/2);
                if(A == B){
                    count++;
                }else break;
            }
            if(count > max_length){
                center = i;
                max_length=count;
            }
            
        }

		// remove imaginary delimiters
        int start = (center - max_length)/2;
        String result = s.substring(start,start+max_length);  
        
        return result;
    }
}
```

### Manacher Algorithm optimize the calculation of helper[] 
Time complexity O($n$)
Space complexity O($n$)
```
class Solution {
	// manlancher
    public String longestPalindrome(String s) {
        if(s.length() <= 1) return s;
        int len = s.length();
        int center = 0;
        int max_length = 0;
        
        int[] helper = new int[2*len+1];
        
	// keep track of the rightest elements we have checked.
	//and its corresponding center
        int max_right = 0;
        int cor_center = 0;
            
        for(int i = 1; i < helper.length; i++){
            int count = 0;
            
            if(i < max_right) {
                int mirror = 2*cor_center - i;     
                // 3 differeent conditions:          
                if(helper[mirror] < max_right - i) count = helper[mirror];
                else if(helper[mirror] > max_right - i) count = max_right - i;
                else{
                    count = max_right - i;
                    for(int j = max_right - i + 1; i-j>=0 && i+j < helper.length; j++){
                        if((i-j)%2 == 0){
                            count++;
                            continue;
                        }
                        char A = s.charAt((i-j-1)/2);
                        char B = s.charAt((i+j-1)/2);
                        if(A == B){
                            count++;
                        }else break;
                    }
                }
            }else{
                for(int j = 1; i-j>=0 && i+j < helper.length; j++){
                    if((i-j)%2 == 0){
                        count++;
                        continue;
                    }
                    char A = s.charAt((i-j-1)/2);
                    char B = s.charAt((i+j-1)/2);
                    if(A == B){
                        count++;
                    }else break;
                }
            }
            
            helper[i] = count;
            // update max_right and corresponding center
            if(i+count > max_right){
                max_right = i+count;
                cor_center = i;
            }
            // update max palindromic string we have found
            if(count > max_length){
                center = i;
                max_length=count;
            }
            
        }

        int start = (center - max_length)/2;
        String result = s.substring(start,start+max_length);
        
        return result;

    }
}

```
