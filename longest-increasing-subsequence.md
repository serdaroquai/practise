## Longest Increasing Subsequence
https://leetcode.com/problems/longest-increasing-subsequence/

> Given an unsorted array of integers, find the length of longest increasing subsequence.
> 
> Example:
> 
> Input: [10,9,2,5,3,7,101,18]  
> Output: 4  
> Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.   
> 
> There may be more than one LIS combination, it is only necessary for you to return the length.  
> Your algorithm should run in O(n2) complexity.  
> Follow up: Could you improve it to O(n log n) time complexity?
---
* To brute force this, you need all subsequences (combinations) that are increasing and find the longest one. `O(n!)`  
* Note that it is only the *length*, we require not the actual result.  
* Let `dp[i]` denote the length of the longest increasing subsequence that ends with the `i`th element. We can base `dp[i]` on solutions of previous `dp[j]` where `j < i`.  
* Idea: if `nums[i] > nums[j]` then `dp[i]` is either `dp[j]+1` (length of longest incr subsequence ending at j plus one) or `dp[i]` whichever is longer. Time-complexity `O(n^2)`, Space-complexity `O(n)`

```java
public int lengthOfLIS(int[] nums) {
  
  //         j  i
  // [10, 9, 2, 5 ,3 ,7,101,18]  nums[]
  // [ 1, 1, 1, 1, 1, 1,  1, 1]  dp[] before
  // [ 1, 1, 1, 2, 1, 1,  1, 1]  dp[] after
        
  int max = 1, len= nums.length;
  int[] dp = new int[len];
  Arrays.fill(dp, 1); // initially every element is a subsequence of length 1 by itself.

  for (int i=1; i<len; i++) {
    for (int j=0; j<i; j++) {
      if (nums[i] > nums[j]) dp[i] = Math.max(dp[i], dp[j]+1);
    }
    max = Math.max(max, dp[i]);
  }

  return max;
}
```

**TODO `O(nlogn)`**

