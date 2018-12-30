# Decode Ways (Medium)
https://leetcode.com/problems/decode-ways/

> A message containing letters from A-Z is being encoded to numbers using the following mapping:  
>
>'A' -> 1  
>'B' -> 2  
>...  
>'Z' -> 26  
>Given a non-empty string containing only digits, determine the total number of ways to decode it.  
>
>Example 1:  
>
>Input: "12"  
>Output: 2  
>Explanation: It could be decoded as "AB" (1 2) or "L" (12).  
>Example 2:  
>
>Input: "226"  
>Output: 3  
>Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).  
---
* Mighty dynamic programming
* An empty string has only one way to decode hence `dp[0] = 1`
* A single digit has only one way to decode unless it is invalid `dp[1] = s.charAt(0) != '0' ? 1 : 0`
* Now there are two paths to link previous solutions 
  * Check last two digits, if it represents a valid number then `dp[i+1] += dp[i-1]`
  * Check last digit, if it represents a valid number then 'dp[i+1] += dp[i]'
* Runtime O(n), Space O(n) n being string length

```java
// dp[0]  ''      ''  
// dp[1]  '1'     A  
// dp[2]  '12'    AB, L  
// dp[3]  '123'   ABC, AW, LC  
// ...3 is valid encoding for leter C so dp[3] += dp[2] (AB + C, L + C)  
// ...23 is valid encoding for leter W so dp[3] += dp[1] (A + W)  

public int numDecodings(String s) {

    int[] dp = new int[s.length()+1];
    dp[0] = 1; 
    dp[1] = s.charAt(0) != '0' ? 1 : 0;

    for (int i=1; i < s.length(); i++) {

        int num = s.charAt(i-1) - '0';
        num = num*10 + s.charAt(i) - '0';

        if (num >= 10 && num <= 26) dp[i+1] += dp[i-1]; // last two digits represents a valid letter
        if (s.charAt(i) != '0') dp[i+1] += dp[i];       // last digit represents a valid letter
    }

    return dp[s.length()];
}
```

* We can further improve space complexity two **O(1)** since solution only depends on two previous solutions. In order to do that we only need two variables `int twoPrev` and `int prev` and a temporary variable `int next` to hold the next answer.

```java
public int numDecodings(String s) {

    int twoPrev = 1; 
    int prev = s.charAt(0) != '0' ? 1 : 0;
    int next;

    for (int i=1; i < s.length(); i++) {

        next = 0;
        int num = s.charAt(i-1) - '0';
        num = num*10 + s.charAt(i) - '0';

        if (num >= 10 && num <= 26) next += twoPrev;
        if (s.charAt(i) != '0') next += prev;

        twoPrev = prev;
        prev = next;
    }

    return prev;
} 
```

# Decode Ways II (Hard)
https://leetcode.com/problems/decode-ways-ii/

> A message containing letters from A-Z is being encoded to numbers using the following mapping way:  
>
>'A' -> 1  
>'B' -> 2  
>...  
>'Z' -> 26  
> Beyond that, now the encoded string can also contain the character '*', which can be treated as one of the numbers from 1 to 9.  
>
> Given the encoded message containing digits and the character '*', return the total number of ways to decode it.  
>
> Also, since the answer may be very large, you should return the output mod 109 + 7.  
>
>Example 1:  
>Input: "*"  
>Output: 9  
>Explanation: The encoded message can be decoded to the string: "A", "B", "C", "D", "E", "F", "G", "H", "I".  
>Example 2:  
>Input: "1*"  
>Output: 9 + 9 = 18  
>Note:  
>The length of the input string will fit in range [1, 105].  
>The input string will only contain the character '*' and digits '0' - '9'.  
---
* Introducing the wild card introduces following changes:
  * since `*` can be a placeholder for many values, number of next possible solutions are multiplied by the number of different values `*` can take in a given situation.
  * Single digit cases:
    * `*` = 9 different values (1,2,3,4,5,6,7,8,9)  `next = next + prev * 9`
  * Double digit cases:
    * `**` = 15 different values, remember * can not be zero by problem definition. (11,12,13,14,15,16,17,18,19,21,22,23,24,25,26) `next = next + prev * 15`
    * `*1` = 2 differnt values (11,21)
    * `*7` = 1 value (17) since 27 > 26 and is not valid
    * `1*` = 9 values (11,12,13,14,15,16,17,18,19)
    * `2*` = 6 values (21,22,23,24,25,26)
* Runtime O(n), Space O(1)

```java
public int numDecodings(String s) {
    // dp[0] ''     = ''
    // dp[1] '1'    = A
    // dp[2] '1*'   = J,K,L,M,N,O,P,Q,R,S + AA,AB,AC,AD,AE,AF,AG,AH,AI

    int mod = 1000000007;

    long twoPrev = 1;
    long prev = (s.charAt(0) == '*') ? 9 : (s.charAt(0) != '0') ? 1 : 0;

    for (int i=1; i<s.length(); i++) {
        long next = 0;

        char c = s.charAt(i);
        char p = s.charAt(i-1);

        // single digit
        if (c == '*') next = (next + prev * 9);
        else if (c != '0') next = next + prev;

        // double digit
        // ** , *1, *7, 1*, 2*, 10..26
        if (p == '*' && c == '*') next = next + (twoPrev * 15);
        else if (p == '*' && Character.isDigit(c) && c - '0' < 7) next = next + (twoPrev * 2);
        else if (p == '*' && Character.isDigit(c) && c - '0' >= 7) next = next + twoPrev;
        else if (p == '1' && c == '*') next = next + (twoPrev * 9);
        else if (p == '2' && c == '*') next = next + (twoPrev * 6);
        else if (Character.isDigit(p) && Character.isDigit(c)) {
            int num = (p -'0')*10 + (c - '0');
            if (num >= 10 && num <=26) next = next + twoPrev;
        }

        next %= mod;
        twoPrev = prev;
        prev = next;
    }

    return new Long(prev).intValue();

}

```
