# 556. Next Greater Element III 31
Given a positive integer  `n`, find  _the smallest integer which has exactly the same digits existing in the integer_  `n`  _and is greater in value than_  `n`. If no such positive integer exists, return  `-1`.

**Note**  that the returned integer should fit in  **32-bit integer**, if there is a valid answer but it does not fit in  **32-bit integer**, return  `-1`.

**Example 1:**

**Input:** n = 12
**Output:** 21

**Example 2:**

**Input:** n = 21
**Output:** -1

**Constraints:**

-   `1 <= n <= 231  - 1`

# Solution 1: Next Permutation (Beat 100%)
```
class Solution {
    public int nextGreaterElement(int n) {
        if(n <= 10) return -1;
        char[] word = new String(n+"").toCharArray();
        
        int last = word.length - 2;
        while(last >= 0 && word[last] >= word[last+1]) last--;
        if(last < 0) return -1;
        
        int larger = word.length - 1;
        while(larger >= 0 && word[larger] <= word[last]) larger--;
        
        swap(word, larger, last);
        reverse(word, last+1);
        
        return parse_arr_to_int(word);
    }
    
    public int parse_arr_to_int(char[] arr){
        int res = 0;
        for(char num: arr){
            int tmp = res*10 + (num - '0');
            if(tmp/10 != res) return -1;
            res = tmp;
        }
        return res;
    }
    
    public void reverse(char[] arr, int a){
        int end = arr.length - 1;
        while(a < end){
            swap(arr,a,end);
            a++;end--;
        }
    }
    
    public void swap(char[] word, int a, int b){
        char tmp = word[a];
        word[a] = word[b];
        word[b] = tmp;
    }
}
```