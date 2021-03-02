# 187. Repeated DNA Sequences
All DNA is composed of a series of nucleotides abbreviated as  `'A'`,  `'C'`,  `'G'`, and  `'T'`, for example:  `"ACGAATTCCG"`. When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.

**Example 1:**

**Input:** s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"
**Output:** ["AAAAACCCCC","CCCCCAAAAA"]

**Example 2:**

**Input:** s = "AAAAAAAAAAAAA"
**Output:** ["AAAAAAAAAA"]

**Constraints:**

-   `0 <= s.length <= 105`
-   `s[i]`  is  `'A'`,  `'C'`,  `'G'`, or  `'T'`.


# Solution 1: Straight Forward (Beat 90%)
```
class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        Set seen = new HashSet(), repeated = new HashSet();
        for (int i = 0; i + 9 < s.length(); i++) {
            String ten = s.substring(i, i + 10);
            if (!seen.add(ten))
                repeated.add(ten);
        }
        return new ArrayList(repeated);
    }
}
```


# Solution 2: Rabin-Karp (Beat 100%)
```
class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        Set<Integer> words = new HashSet<>();
        Set<Integer> doubleWords = new HashSet<>();
        List<String> rv = new ArrayList<>();
        if(s.length() <= 10) return rv;
        
        char[] map = new char[26];
        map['C' - 'A'] = 1;
        map['G' - 'A'] = 2;
        map['T' - 'A'] = 3;

        int v = 0;
        
        for(int j = 0; j <  10; j++) {
            v <<= 2;
            v |= map[s.charAt(j) - 'A'];
        }
        words.add(v);
        
    
        for(int i = 10; i < s.length(); i++) {
            v &= 0x3ffff;
            v <<= 2;
            v |= map[s.charAt(i) - 'A'];
            
            if(!words.add(v) && doubleWords.add(v)) {
                rv.add(s.substring(i-9, i+1));
            }
        }
        return rv;
    }
}
```