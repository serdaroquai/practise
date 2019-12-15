#### Subarray sum equals K
  * equals K suggests using a map
  * greedy `O(n)`

#### Serialize/Deserialize Binary Tree
  * Just do a preorder and insert # as nulls `O(n)`
  
#### Remove Invalid Parentheses
  * Return all results, and minimum removals => combinations && BFS
  * `(()` remove 0 or 1 both yields same result, so get skip consecutives
  * `(()(()` remove 0 or 3 both yield same result, so keep last position
  * if you remove '(' and then remove `)` best case is you will end up in a sub optimal result
  
#### Integer to English Words
  * <20 <100 <1000 recursive helper

#### Minimum Window Substring
  * frequency map, count

#### Merge Intervals
  * Array must be sorted
  * 2 pointers `O(nlogn)`

#### Merge K Sorted Lists
  * k lists, a total of n elements
  * heap for each non null list head (size k), keep polling until it is over
  * `O(nlogk)`

#### Product of array except self
  * lr, rl multiplications
  * `result[i] = lr[i-1] * rl[i+1]`
  * `O(n)`

### Takeaways
* Keep PriorityQueue's size limited to k instead of n so that run time is `O(nlogk)` instead of `O(nlogn)`
* Can use a Queue<> of tokens to pass down to a recursive function in order to track position
