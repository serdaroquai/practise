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
