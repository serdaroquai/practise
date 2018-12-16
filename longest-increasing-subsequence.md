## Longest Continuous Increasing Subsequence
https://leetcode.com/problems/longest-continuous-increasing-subsequence/

> Given an unsorted array of integers, find the length of longest continuous increasing subsequence (subarray).
> 
> Example 1:  
> Input: [1,3,5,4,7]  
> Output: 3  
> Explanation: The longest continuous increasing subsequence is [1,3,5], its length is 3.  
> Even though [1,3,5,7] is also an increasing subsequence, it's not a continuous one where 5 and 7 are separated by 4.
> 
> Example 2:  
> Input: [2,2,2,2,2]  
>  Output: 1  
> Explanation: The longest continuous increasing subsequence is [2], its length is 1.  
> Note: Length of the array will not exceed 10,000.
---
* Brute forcing this would be for every element, traverse rest of the elements to find the LCIS. `O(n^2)`
* Current length of LCIS depends only on previous element. If `nums[i] > nums[i-1]`then `length++`, else `length=1`
* Greedy. Time `O(n)`, Space `O(1)`

```java
public int findLengthOfLCIS(int[] nums) {
  if (nums == null || nums.length == 0) return 0;

  int max = 1, len = 1;

  for (int i=1; i< nums.length; i++) {
    if (nums[i] > nums[i-1]) max = Math.max(max, ++len);
    else len = 1;
  }

  return max;
}
```

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
public int findLengthOfLCIS(int[] nums) {
  if (nums == null || nums.length == 0) return 0;

  int max = 1, len = 1;

  for (int i=1; i< nums.length; i++) {
    if (nums[i] > nums[i-1]) { 
      len++;
      max = Math.max(max, len);
    } else {
      len = 1;
    }
  }

  return max;
}
```

**TODO `O(nlogn)`** Patience algorithm

