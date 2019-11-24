### anagrams of one string exists in other string

```java
int[] freq = new int[26]; // frequency of each letter
int count = 0;            // number of distinct letters
```

iterate over string once to fill both arrays.  
`if (freq[c-'a'] == 0) count++;`
iterate over other string to find out if count is 0.  
`if (freq[c-'a] == 1) count--;`

> Edge case: check length of both strings are the same since this does not cover strings of different lengths

### Longest palindrome substring

* `String : [cbcbbcda]`
* For each single or tuple expansion point (a plaindrome can be odd or even length, latter having two center chars)
* expand and find the longest. `O(n^2)`

### Longest palindrome subsequence

* `2^n` either pick or not pick, `n^3` pick each center and try expanding, `n^2` dp solution
* dp uses the same expansion principle
* There are two cases
  * longest becomes +2 if chars surrounding the substring are equal.
  * else it is the best of what we have so far

```
public int longestPalindromeSubseq(String s) {
        int l = s.length();
        
        // dp[i][j] represents the longest palindrome in the substring 
        // starting with char at index i, and ends with the char at index j;
        int[][] dp = new int[l][l];
        
        /* what dp [i][j] represents
                j
            \   c    b    b    d
         i  c   c    cb   cbb  cbbd
            b        b    bb   bbd
            b             b    bd
            d                  d
        
        */
        
        /* we want to iterate this way to simulate expanding
                j
            \   c    b    b    d
         i  c   1  → ↑    ↑    ↑
            b        1  → ↑    ↑
            b             1  → ↑
            d                  1        
        
        */
        
        // all single chars (diagonal) are palindromes
        // also all i > j are invalid strings therefore they have 0 palindromes naturally
        for (int i=0; i<l; i++) dp[i][i] = 1; 
        
        for (int j=0; j<l; j++) {
            for (int i=j-1; i>=0; i--) {
                if (s.charAt(j) == s.charAt(i)) dp[i][j] = dp[i+1][j-1] + 2;
                else dp[i][j] = Math.max(dp[i+1][j], dp[i][j-1]);
            }
        }
        
        return dp[0][l-1];
    }
```
