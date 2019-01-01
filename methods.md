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

* Keep a running sum with two pointers
  * `sum += nums[r++]` increment right pointer and increase sum
  * `while (sum >= s)` update best result and `sum -= nums[l++]`, decrease sum
  
## Shortest subarray with sum at least K
https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/

* Make a sum array such that `sum[i] = A[0] + A[1] + .. + A[i-1]`
* What makes this problem hard (and not solved by two pointers is negative numbers)
* At this point if you pick every pair of elements and check if their sum is >= K you find your answer in `O(n^2)`
* Create a Deque `d`. We will use it to store **indices** (we need the posiiton) of increasing sums. Two intuitions:
    - As soon as we reach a point where `sums[i] - sums[d.getFirst()] >= K` we will start removing from the front of the deque to find the minimum `(i,j)` index pair that satisfies the condition. This is the same as using a sliding window. Since we only store increasing sums this will work. When we find a better minimum for current `i` note that for a greater `i` same solution can never be the minimum again. Therefore we remove it.
    - When `sums[i] <= sums[d.getLast()]` in that case `sums[d.getLast()]` can never be a minimum solution. since the solution before it is a higher sum with a lower index. So we only need the last element (`sums[i]`) of a decreasing sequence. Therefore remove previous element if the sum is same or decreasing.
* Every element gets in the queue once and gets out once. Therefore time complexity is `O(n)`
```java
...
for (int i=0; i<l+1; i++) {
    while (!d.isEmpty() && sums[i] - sums[d.getFirst()] >= K) min = Math.min(min, i - d.pollFirst());
    while (!d.isEmpty() && sums[i] <= sums[d.getLast()]) d.pollLast();
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
