# 949. Largest Time for Given Digits
Given an array `arr`  of 4 digits, find the latest 24-hour time that can be made using each digit  **exactly once**.

24-hour times are formatted as  `"HH:MM"`, where  `HH` is between `00` and `23`, and `MM` is between `00` and `59`. The earliest 24-hour time is  `00:00`, and the latest is  `23:59`.

Return  _the latest 24-hour time in `"HH:MM"`  format_. If no valid time can be made, return an empty string.

**Example 1:**

**Input:** A = [1,2,3,4]
**Output:** "23:41"
**Explanation:** The valid 24-hour times are "12:34", "12:43", "13:24", "13:42", "14:23", "14:32", "21:34", "21:43", "23:14", and "23:41". Of these times, "23:41" is the latest.

**Example 2:**

**Input:** A = [5,5,5,5]
**Output:** ""
**Explanation:** There are no valid 24-hour times as "55:55" is not valid.

**Example 3:**

**Input:** A = [0,0,0,0]
**Output:** "00:00"

**Example 4:**

**Input:** A = [0,0,1,0]
**Output:** "10:00"

**Constraints:**

-   `arr.length == 4`
-   `0 <= arr[i] <= 9`

# Solution 1: Exhaustive Search (Beat 81%)
```
class Solution {
    public String largestTimeFromDigits(int[] A) {
        String ans = "";
        int maxh = -1;
        int maxm = -1;
        for(int i = 0; i <= 3; i++){
            for(int j = 0;j <= 3; j++){
                for(int k = 0; k <= 3; k++){
                    for(int m = 0; m <= 3; m++){
                        if(i == j || i == k || i == m || j == m || k == m || j == k) continue;
                        int hours = A[i]*10 + A[j];
                        int minutes = A[k]*10 + A[m];
                        
                        if(hours < 24 && minutes < 60 && (hours > maxh || ( hours == maxh && minutes >= maxm) ) ){
                            maxm = minutes;
                            maxh = hours;
                        }
                    }
                }
            }
        }
        if(maxh == -1) return ans;
        if(maxh < 10) ans = ans + "0";
        ans = ans + maxh + ":";
        if(maxm < 10) ans = ans + "0";
        ans = ans + maxm;
        
        return ans;
    }
}
```