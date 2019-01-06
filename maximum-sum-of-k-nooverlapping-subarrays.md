## Maximum sum of k non overlapping sub arrays
Not to be confused with : https://leetcode.com/problems/maximum-sum-of-3-non-overlapping-subarrays/ (original question asks starting indices and is limited to k=3)

* nums conists of only **positive** integers
* If we were asked to just return max sum of m contiguous non overlapping subarrays with length k each.
* Things to learn here are:
  * Prefix sum, saves O(k) time for calculating sum each time
  * Dynamic programming,

```java
// nums     [ 1, 2, 1, 2, 6, 7, 5, 1]   length N
// sum    [0, 1, 3, 4, 6,12,19,24,25]   sum of nums[0..i] length N+1
// kSum   [0, 0, 3, 3, 3, 8,13,12, 6]   ex k=2; sum of k length subarray ending at i. sum[i] - sum[i-k]
// maxSum [0, 0, 3, 3, 3, 8,13,13,13]   max sum of k length subarray between [0..i] (for 1 array)
// maxSum [0, 0, 3, 3, 6,11,16,20,20]   (for 2 arrays)
// maxSum [0, 0, 3, 3, 6,11,19,23,23]   (for 3 arrays)
// return maxSum[N]

public int[] maxSumOfThreeSubarrays(int[] nums, int k) {
  int N = nums.length;
  int m = 3; // number of arrays

  // sum arr
  int[] sum = new int[N+1];
  for (int i=0; i<N; i++) sum[i+1] = sum[i] + nums[i];

  // sum of contigious subArr of length k that ends at i
  int[] kSum = new int[N+1];
  for (int i=k; i<N+1; i++) kSum[i] = sum[i] - sum[i-k];

  // max sum between [0,i] (for a single arr)
  int[] maxSum = new int[N+1];
  for (int i=1; i<N+1; i++) maxSum = Math.max(kSum[i], maxSum[i-1]);

  // updating maxSum starting from 2nd arr of m arrays
  for (int i=2; i <= m; i++) {
      int[] nextMaxSum = new int[N+1];
      
      // smallest index ith arr can end at is i*k therefore start there
      for (int j=i*k; j<N+1; j++) {
          // is max of "last subarray ending at j + max sum for all prev subarrays before start of last subarray (j-k)" OR
          // best solution between [0..j-1] 
          nextMaxSum = Math.max(kSum[j] + maxSum[j-k], nextMaxSum[j-1]);
      }
      maxSum = nextMaxSum;
  }
  
  return maxSum[N];
}
```
