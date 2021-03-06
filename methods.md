## Maximum Sum Subarray
https://leetcode.com/problems/maximum-subarray/

[-2,1,-3,4,-1,2,1,-5,4]  

* Keep a running sum
* At each new element 
  * `sum = Math.max(sum + nums[i], nums[i])` : Discard old running sum if nums[i] is bigger.
  * `best = Math.max(best, sum)` : Update if best result
  
## Minimum Size Subarray Sum
https://leetcode.com/problems/minimum-size-subarray-sum/

> s = 7, nums = [2,3,1,2,4,3] (only positive integers)    
> Output:2  

* Keep a running sum with two pointers **(works because nums has only positive integers)**
  * `sum += nums[r++]` increment right pointer and increase sum
  * `while (sum >= s)` update best result and `sum -= nums[l++]`, decrease sum
  
## Maximum Size Subarray Sum Equals k
https://leetcode.com/problems/maximum-size-subarray-sum-equals-k

> Input: nums = [1, -1, 5, -2, 3], k = 3
> Output: 4 
> Explanation: The subarray [1, -1, 5, -2] sums to 3 and is the longest.

* Keep a running sum, and initialize a map of <seen sums so far, and their indices> (initialize map with sum:0, indice:-1)
* At each element
  * Since `k = sum - someoldSum` check if `someoldSum = sum - k` exists in map, if it does `best = Math.max(best, map.get(sum -k))`
  * if map does not contain `sum` add it else keep the old indice (since its guaranteed to be smaller)
  
## Shortest subarray with sum at least K
https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/

* What makes this problem hard (and not solved by two pointers is negative numbers)
* Make a sum array such that `sum[i] = A[0] + A[1] + .. + A[i-1]`
* Because of positive/negative numbers our sum array is not monotonically increasing/decreasing hence for each sum we need to check all previous elements.
* If instead of `sum >= k` we had `sum == k`, then we could get away with a map of previous sums like *Maximum Size Subarray Sum Equals k* in `O(n)` time.
* At this point if we pick every pair of elements and check if their sum difference is >= K we find our answer in `O(n^2)`
* Create a Deque `d` to store **indices** (we need the posiiton) of increasing sums. Two intuitions:
    - while we are checking for valid solutions, if `sums[i] - sums[d.peekFirst()]` is a valid solution, it is always going to be a shorter solution than `sums[i+1] - sums[d.peekFirst()]`, so we can safely remove head of deque.
    - while we are offering the latest i to the queue, if `sums[d.peekLast()] >= sums[i]` in that case `sums[d.getLast()]` can never be a minimum solution for a future `i` since `sums[i]` is a lower sum with a higher index, it will always be a better solution, therefor we can remove it.
* Every element gets in the queue once and gets out once. Therefore time complexity is `O(n)`
```java
...
for (int i=0; i<l+1; i++) {
    while (!d.isEmpty() && sums[i] - sums[d.peekFirst()] >= K) min = Math.min(min, i - d.pollFirst());
    while (!d.isEmpty() && sums[d.getLast()] >= sums[i]) d.pollLast();
    d.addLast(i);
}
```
  
## Minimum Window Substring
https://leetcode.com/problems/minimum-window-substring/

> Input: S = "ADOBECODEBANC", T = "ABC"  
> Output: "BANC"  

* This is a minimum size subarray sum problem.
  * Instead of keeping a running sum, we keep a running count on char frequency map
    ```java
    int[] freq = new int[128]; // number of times each char appears
    int count = 0;             // number of distinct chars
    for (char c : t.toCharArray()) freq[c]++;
    for (int i : freq) count += (i > 0) 1 : 0;
    ```
  * increment right pointer and update `freq` and `count`
    ```java
    if (freq[c] == 1) count--;  // no more of that char left
    freq[c]--;
    ```
  * `while (count==0) ` update best result, `freq` and `count`.
    ```java
    if (freq[c] == 0) count++;  // uh-oh now we need that char
    freq[c]++;
    ```
