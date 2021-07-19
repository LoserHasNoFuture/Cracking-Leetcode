# 927. Three Equal Parts
You are given an array  `arr`  which consists of only zeros and ones, divide the array into  **three non-empty parts**  such that all of these parts represent the same binary value.

If it is possible, return any  `[i, j]`  with  `i + 1 < j`, such that:

-   `arr[0], arr[1], ..., arr[i]`  is the first part,
-   `arr[i + 1], arr[i + 2], ..., arr[j - 1]`  is the second part, and
-   `arr[j], arr[j + 1], ..., arr[arr.length - 1]`  is the third part.
-   All three parts have equal binary values.

If it is not possible, return  `[-1, -1]`.

Note that the entire part is used when considering what binary value it represents. For example,  `[1,1,0]`  represents  `6`  in decimal, not  `3`. Also, leading zeros  **are allowed**, so  `[0,1,1]`  and  `[1,1]`  represent the same value.

**Example 1:**

**Input:** arr = [1,0,1,0,1]
**Output:** [0,3]

**Example 2:**

**Input:** arr = [1,1,0,1,1]
**Output:** [-1,-1]

**Example 3:**

**Input:** arr = [1,1,0,0,1]
**Output:** [0,2]

**Constraints:**

-   `3 <= arr.length <= 3 * 104`
-   `arr[i]`  is  `0`  or  `1`

# Solution:
```
class Solution {
    public int[] threeEqualParts(int[] arr) {
        int sum = getNumOfOnes(arr);
        if(sum%3 != 0) return new int[]{-1,-1};
        if(sum == 0) return new int[]{0,arr.length-1};
        
        int index = arr.length - 1, cnt = sum/3;
        for(; index >= 0 && cnt > 0; index--){
            if(arr[index] == 1) cnt--;
        }
        int lastStart = index + 1;
        
        int second = getStartPosition(arr, lastStart, index, sum/3);
        if(second == -1) return new int[]{-1,-1};
        
        int first = getStartPosition(arr, lastStart, second-1, sum/3);
        if(first == -1) return new int[]{-1,-1};
        
        int patternSize = arr.length - lastStart;
        return new int[]{first+patternSize - 1, second + patternSize};
    }
    
    public int getStartPosition(int[] arr, int last, int end, int cnt){
        int index = end, tmp = cnt;
        for(; index >= 0 && cnt > 0; index--){
            if(arr[index] == 1) cnt--;
        }
        index++;
        
        if(end + 1 - index < arr.length - last) return -1;
        
        for(int i =  index, j = last; tmp > 0; i++, j++){
            if(arr[i] == 1) tmp--;
            if(arr[i] != arr[j]) return -1;
        }
        
        return index;
    }
    
    
    public int getNumOfOnes(int[] arr){
        int sum = 0;
        for(int num: arr){
            if(num == 1) sum++;
        }
        return sum;
    }
    
}
```