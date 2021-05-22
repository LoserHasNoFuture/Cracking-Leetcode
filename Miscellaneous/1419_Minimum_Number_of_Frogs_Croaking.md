# 1419. Minimum Number of Frogs Croaking
Given the string  `croakOfFrogs`, which represents a combination of the string "croak" from different frogs, that is, multiple frogs can croak at the same time, so multiple “croak” are mixed. _Return the minimum number of_ different _frogs to finish all the croak in the given string._

A valid "croak" means a frog is printing 5 letters ‘c’, ’r’, ’o’, ’a’, ’k’ **sequentially**. The frogs have to print all five letters to finish a croak. If the given string is not a combination of valid "croak" return -1.

**Example 1:**

**Input:** croakOfFrogs = "croakcroak"
**Output:** 1 
**Explanation:** One frog yelling "croak**"** twice.

**Example 2:**

**Input:** croakOfFrogs = "crcoakroak"
**Output:** 2 
**Explanation:** The minimum number of frogs is two. 
The first frog could yell "**cr**c**oak**roak".
The second frog could yell later "cr**c**oak**roak**".

**Example 3:**

**Input:** croakOfFrogs = "croakcrook"
**Output:** -1
**Explanation:** The given string is an invalid combination of "croak**"** from different frogs.

**Example 4:**

**Input:** croakOfFrogs = "croakcroa"
**Output:** -1

**Constraints:**

-   `1 <= croakOfFrogs.length <= 10^5`
-   All characters in the string are:  `'c'`,  `'r'`,  `'o'`,  `'a'`  or  `'k'`.

# Solution 1: State Machine (Beat 65%)
[https://leetcode.com/problems/minimum-number-of-frogs-croaking/discuss/586543/C%2B%2BJava-with-picture-simulation](https://leetcode.com/problems/minimum-number-of-frogs-croaking/discuss/586543/C%2B%2BJava-with-picture-simulation)
Count the number of current singing frog.
And the maximum is the total number of frog.
```
class Solution {
    public int minNumberOfFrogs(String croakOfFrogs) {
        int cnt[] = new int[5];
        int frogs = 0, max_frogs = 0;
        for (var i = 0; i < croakOfFrogs.length(); ++i) {
            var ch = croakOfFrogs.charAt(i);
            var n = "croak".indexOf(ch);
            ++cnt[n];
            if (n == 0)
                max_frogs = Math.max(max_frogs, ++frogs);
            else if (--cnt[n - 1] < 0)
                return -1;
            else if (n == 4)
                --frogs;
        }
        return frogs == 0 ? max_frogs : -1;    
    }
}
```
