### Integer to English Words
  * <20 <100 <1000 recursive helper

### Minimum Window Substring
  * frequency map, count

### Merge Intervals
  * Array must be sorted
  * 2 pointers `O(nlogn)`

### Merge K Sorted Lists
  * k lists, a total of n elements
  * heap for each non null list head (size k), keep polling until it is over
  * `O(nlogk)`

### Product of array except self
  * lr, rl multiplications
  * `result[i] = lr[i-1] * rl[i+1]`
  * `O(n)`

## Takeaways
* Keep PriorityQueue's size limited to k instead of n so that run time is `O(nlogk)` instead of `O(nlogn)`
