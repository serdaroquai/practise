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
