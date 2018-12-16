## Longest Continuous Increasing Subsequence (Easy)
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

## Longest Increasing Subsequence (Medium)
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

**TODO `O(nlogn)`** Patience algorithm

## Number of Longest Increasing Subsequence (Medium)
https://leetcode.com/problems/number-of-longest-increasing-subsequence/

> Given an unsorted array of integers, find the number of longest increasing subsequence.  
> 
> Example 1:  
> Input: [1,3,5,4,7]  
> Output: 2  
> Explanation: The two longest increasing subsequence are [1, 3, 4, 7] and [1, 3, 5, 7].  
> 
> Example 2:  
> Input: [2,2,2,2,2]  
> Output: 5  
> Explanation: The length of longest continuous increasing subsequence is 1, and there are 5 subsequences' length is 1, so output 5.  
> 
> Note: Length of the given array will be not exceed 2000 and the answer is guaranteed to be fit in 32-bit signed int.  
---
* This extends Length of LIS, by keeping the `count[i]` that represents count of LIS ending at index `i`.
* If `nums[i] > nums[j] && dp[j]+1 > dp[i]` that means we have a new best solution for index `i`.
  * In that case `count[i] = count[j]`
* If `nums[i] > nums[j] && dp[j]+1 == dp[i]` that means we have an alternative best solution for index `i`.
  * In that case `count[i] += count[j]`
* At each iteration of `i` update the result.
* Time `O(n^2)`, Space `O(n)`

```java
public int findNumberOfLIS(int[] nums) {
    if (nums == null || nums.length == 0) return 0;

    int max = 1, result = 1, len = nums.length;
    int[] dp = new int[len];
    int[] count = new int[len];
    Arrays.fill(dp,1);
    Arrays.fill(count,1);

    for (int i=1; i<len; i++) {
        for (int j=0; j<i; j++) {
            if (nums[i] > nums[j]) { // nums[i] can extend nums[j] to make a longer increasing subsequence
                if (dp[j]+1 == dp[i]) { // we have an alternative solution to our best solution
                    count[i] += count[j]; // add to total counts
                } else if (dp[j] + 1 > dp[i]) { // we have a new best solution
                    dp[i] = dp[j] + 1; // update best solution length
                    count[i] = count[j]; // reset counts to best solution
                }
            }
        }

        if (dp[i] == max) { // local solution as good as global best
            result += count[i]; // add to total global best solution count
        } else if (dp[i] > max) { // local solution better than global best
            max = dp[i]; // update global best
            result = count[i]; // set new count as global best
        };
    }

    return result;
}
```

**TODO `O(nlogn)`** Segment trees
