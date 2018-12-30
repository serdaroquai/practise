# Minimum Window Subsequence (Hard)
https://leetcode.com/problems/minimum-window-subsequence/

>Given strings S and T, find the minimum (contiguous) substring W of S, so that T is a subsequence of W.
> 
>If there is no such window in S that covers all characters in T, return the empty string "". If there are multiple such minimum-length windows, return the one with the left-most starting index.
>
>Example 1:
>
>Input:  
>S = "abcdebdde", T = "bde"  
>Output: "bcde"  
>
>Explanation:  
>"bcde" is the answer because it occurs before "bdde" which has the same length.  
>"deb" is not a smaller window because the elements of T in the window must occur in order.  
> 
>
>Note:  
>
>All the strings in the input will only contain lowercase letters.  
>The length of S will be in the range [1, 20000].  
>The length of T will be in the range [1, 100].  
---

* Naive solution is O(S^2) worst case with O(1) memory
  * for each i in S if S.charAt(i) == T.charAt(i) start two pointer check on S & T in order to find the shortest match.
  * sucks for cases like `S="aaaaaaaaaaaaaaaaaaaa"` and `T="ab"`

* A smarter solution is to use two pointers in a different way.
  * Intuition is given some subsequence [a**bcde**bd] you can trim both ends if you approach it greedy from both directions.
  * first start a two pointer check to find any subsequence of S that contains T
```java
First step Initial:
//   s -->
// [ a b c d e b d d e ]
//
//   t -->
// [ b d e ]

First step Final: subsequence is [a,b,c,d,e] after trimmed remaining part of [b,d,d,e]
//           s
// [ a b c d e b d d e ]
//
//       t
// [ b d e ]

```
  * Once you find a subsequence, mark the end, and this time two pointer it in reverse direction to find the shortest subsequence
  * s and t are off by -1, (just an implementation detail)
  * Update the shortest subsequence and keep going
```java
Second step Initial:
//       <-- s
// [ a b c d e b d d e ]
//           e
//   <-- t
// [ b d e ]

Second step Final: subsequence is [b,c,d,e] shortest after trimming prior part of [a]
//   s
// [ a b c d e b d d e ]
//           e
// t
// [ b d e ]

Moving on:
//       s -->
// [ a b c d e b d d e ]
//           
//   t -->
// [ b d e ]
```


```java
public String minWindow(String S, String T) {
    int s = 0, t = 0;
    int start = -1, len = S.length() + 1;
    
    while (s < S.length()) {
        if (S.charAt(s) == T.charAt(t)) {
            if (t == T.length() - 1)  { // actually it is t = len but since t is not incremented yet..
                int end = s;
                while (t >= 0) {
                    while (S.charAt(s) != T.charAt(t)) s--;
                    s--; t--;
                }
                // t= -1 here, s is also off by -1
                if (end-s < len) {
                    start = s+1;
                    len = end-s;
                }
                s++;
            }
            t++;
        }
        s++;
    }

    return len > S.length() ? "" : S.substring(start,start+len);
}
```

* Dynamic Programming. transfering starting index
  * let `dp[t][s] = i` mean starting index of S, where `S.substring(i,s)` matches `T.substring(0,t)`
  * size of dp is `dp[Tlen+1][Slen+1]` in order to represent empty strings at dp[0][0]. Since because of this t and s will stat from 1 and be off by +1, `T.charAt(t-1) == S.charAt(s-1)` actually compares current characters not previous.
  * if we iterate S for each letter of T (left to right) rules to transfer state are :
    * if `T.charAt(t-1) == S.charAt(s-1)` (current characters are same) then carry the starting index of previous `dp[t-1][s-1]`
    * if not then carry `dp[t][s-1]`
* Time complexity O(TS), space O(TS)
    
```java
  Initial:
          a  b  c  d  e  b  d  d  e
       0  1  2  3  4  5  6  7  8  9
    0  0  1  2  3  4  5  6  7  8  9
  b 1 -1 ----------------------------> t=1 s=1..sLen 
  d 2 -1 ----------------------------> t=2 s=1..sLen
  e 3 -1 ----------------------------> 

  Final:
          a  b  c  d  e  b  d  d  e
       0  1  2  3  4  5  6  7  8  9
    0  0  1  2  3  4  5  6  7  8  9
  b 1 -1 -1  1  1  1  1  5  5  5  5
  d 2 -1 -1 -1 -1  1  1  1  5  5  5
  e 3 -1 -1 -1 -1 -1  1  1  1  1  5
```

* state transfer only relies on previous row therefore it is possible to optimize space to O(s)

```java
public String minWindow(String S, String T) {

    int[] prev = new int[S.length()+1];
    for (int i=0; i<prev.length; i++) prev[i] = i;

    for (int t=1; t<=T.length(); t++) {
        int[] next = new int[S.length()+1];
        next[0] = -1; // set start of current row to -1 to mark not found
        for (int s=1; s<=S.length(); s++) {
            next[s] = (S.charAt(s-1) == T.charAt(t-1)) ? prev[s-1] : next[s-1];
        }
        prev = next;
    }
    
    // find minimum length solution
    int start=-1, end=-1, len=S.length()+1;
    for (int i=1; i< prev.length; i++) {
        if (prev[i] != -1 && i-prev[i] < len) {
            start = prev[i]; end = i; len = end-start;
        }
    }

    return len <= S.length() ? S.substring(start,end) : "";
}
```
