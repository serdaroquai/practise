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
// dp[3]  '123'   ABC,AW,LC  
// ...3 is valid leter so dp[3] += dp[2]  
// ...23 is valid leter so dp[3] += dp[1]  

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
