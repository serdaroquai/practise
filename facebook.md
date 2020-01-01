#### Range Sum Query 2D - Immutable
  * use `dp[rl+1][cl+1]` during initialization
  * `dp[r+1][c+1]` denotes the sum of elements in rectangle r,c
  * trick is to define the area in terms of other rectangles.
  * `O(rc)` to initialize `O(1)` to query

#### Vertical Order Traversal
  * find width of tree, calculate offset
  * BFS, and sort each level before adding it to the result

#### Is graph bipartite
  * color 0 unviisted, 1 or -1

#### Exclusive Time of Functions
  * Stack to keep track of processes
  * Trick is that since given instances are timestamps "end" - "start" is actually `end-start+1` long
  * Edge case `0:s:0, 0:e:0, 1:s:1, 1:e:1`

#### Continuous Subarray Sum
  * exact sum = use map,
  * since n is an integer can reduce problem to positive integers + handle zero case separately
  * `O(n)`

#### Reorganize String
  * maxHeap, pull 2 at once `O(nlog(26))`
  * edge case `aaabc`
  * Can do it in `O(n)` by filling even indexes first with most frequent letters, then odd..

#### Add or Search Word
  * Trie `O(m)` to add a word m is length of word
  * recursive dfs `O(26^m)` to search.

#### Intersection Of Two Arrays
  * naive variation is solved by a set
  * Facebook variation is arrays are sorted.
  * two pointers, when num1 equals num2 iterate i and j in while loops to skip duplicates

#### Expression Add Operators
  * keep last term as parameter so we can roll back multiplication.
    * send `-cur` for subtraction and `prev*cur` for multiplication so that we get away without any if blocks  `helper(s, i+1, target, eval-prev + prev*cur, prev*cur, sb, result);` for multiplication
  * Edge cases,
    * use `long` to avoid overflow
    * numbers startig with `0` should stopped. handle this first so we don't accidently skip it for `pos=0` case.
    * case `pos=0` should be handled separately.
    * we don't want to start by appending operators		
  * Runtime `O(3^(n-1))*n`, Space `O(n)` (recursion tree depth)

#### Clone graph
  * Keep a visited map of <oldNode, newNode>
  * Trick is to return cloned node if visited

#### Longest Substring with At most K distinct chars
  * freq map 2 pointers
  * `O(n)`

#### Diameter of Binary Tree
  * bottom up,
  * each step updates best result
  * and returns max branch+1

#### Binary Search Tree Iterator
  * Use a Stack amd iterative in order traversal
  * `hasNext()` becomes `!stack.isEmpty()`
  * stack all the way to the left child during `next()`
  
#### Add Strings
  * Same as add binary

#### Convert Binary Search Tree to Doubly Linked List
  * In order traversal
  * Use a dummy node for prev
  * when iteration ends, prev = last node and dummy.right = head node

#### Simplify Path
  * `s.split("/+")`
  * add non null tokens to deque, and build stringBuilder
  * edge case `/..`
#### Find all anagrams in a string
  * freqMap
  * sliding window, just worry about the chars in freq when it comes to keeping count
  * `O(n)`

#### All nodes distance K in Binary Tree
  * write a helper method `mark` that adds K deep children of a given node.
  * then top down to find target, mark(target,K), and return K-1 (bottom up) so that parents of target can do the same

#### Find first and last position of element
  * Binary search with range predicate
  * `int m=l +(r+1-l)/2` for the case where `l=m`

#### Valid number
  * trim whitespace, then parse char by char and make use of 
    * seenSign, seenDot, seenNumber, seenE
    * Seeing E resets all other
    * return seen number
  * Some interseting edge cases are 3e2.4 false .1 true, 3. true, 3.e2 true

#### Interval List Intersections
  *  intervals are sorted already so 2pointers `O(n)`
  *  after comparing the two if they intersect add it to result,
  *  increment the one with the least end date
  *  intersect helper method can swap interval that starts earlier to the first param for better readability
  
#### Valid Palindrome ||
  * recursion `isValid(String s, int l, int r, int lives)`

#### Add Binary
  * swap longer string to position a,
  * use bitwise instead of / and %
  * sb.reverse() helps

#### Read N Chars give Read4
  * create a buffer with cap c=4, read pointer r=4
  * while n>0
    * consume buffer first (while n>0 && r<c)
    * refill buffer by calling read4 (if n>0 && c==4)
  * return count (local write pointer)
  

#### Validate Binary Search tree
  * recursive (Node n, Node left, Node right), or iterative (in order)

#### Task Scheduler

#### Binary Tree Right Side View
  * BFS, add only last element as result 
  
#### Find Kth largest element
  * Quick select `O(n)`
  * Kth largest also means len - K smallest

#### Valid Palindrome
  * `Character.isLetterOrDigit(c); Character.toLowerCase(c)`
  * Test **edge case** `".,"`

#### Binary Tree Max Path Sum
  * return self + (best or none of both paths)
  * test if self + (any number of branches) makes the best solution
  * Test single node negative value **edge case**
  * `O(n)`

#### Word Break
  * dp denotes s.substring(j,i)
  * for i=1 to <=len, for (j=0 to <i)
  * `dp[i] = dp[j] && dict.contains(s.substring(j,i))`;
  * don't forget to **break** whenever dp[i] is true
  * `O(n^2)`

#### Next Permutation
  * find first decreasing number rl and call it pivot `O(n)`
  * again from rl find the first greater number than pivot and swap them `O(n)`
  * reverse the right of pivot

#### Verifying an alien dictionary
  * given all chars are same smaller word must be shorter

#### K closest points to origin
  * naive way to sort and select Kth, `O(nlogn)`
  * better way is to heap only k elements `O(nlogk)`
  * better way is to quick select `O(n)`

#### Alien Dictionary
  * make sure you include all letters,
  * topo sort, 1 for visiting 2 for visited

#### Meeting Rooms II
  * Sort starts & ends, 2 pointer to keep best result `O(nlogn)`

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
  * **edgecases** 0, 123456, 5000000

#### Minimum Window Substring
  * frequency map, count

#### Merge Intervals
  * Array must be **sorted**
  * 2 pointers `O(nlogn)`

#### Merge K Sorted Lists
  * k lists, a total of n elements
  * heap for each **non null** list head (size k), keep polling until it is over
  * `O(nlogk)`

#### Product of array except self
  * lr, rl multiplications
  * `result[i] = lr[i-1] * rl[i+1]`
  * `O(n)`

### Takeaways
* Keep PriorityQueue's size limited to k instead of n so that run time is `O(nlogk)` instead of `O(nlogn)`
* Can use a Queue<> of tokens to pass down to a recursive function in order to track position
* Swapping parameters
  ```java
  public int someFn(int[] a, int[] b) {
    if (a.length < b.length) return someFn(b,a);
  ```
* Binary search trick `int m= l + (r+1-l)/2`
