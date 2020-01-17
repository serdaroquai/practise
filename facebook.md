#### Basic Calculator II
  * Use a Queue<Character> since java has no tuple, aka recursive calls can not return the index AND the result
  * `(` --> `num = calc(queue)` (sets the num)`
  * rest is same as Basic Calculator

#### Basic Calculator
  * keep last op,
  * each time you encounter an operator, handle last op.
  * execute multiplication and division immediately, rest goes to stack (- as negative number)
  * aggregate and return whats in the stack

#### Word Ladder II
  * build a graph `h#t -> [hot, hit]`
  * Build combinations in nested for loops
    * first step : `[[hot]]`
    * second step: `[[hot,hit], [hot,lit]]`
  * remove words from `wordSet` when you process the word

#### Word Ladder
  * build a graph `h#t -> [hot, hit]`
  * BFS, until you find solution and return depth
  * remove `next word` from `wordSet` while adding to queue (visited) in other words set visited before you visit

#### Shortest Distance from all buildings
  * BFS for each building aggregate all distances in to a `dist[][]` matrix.
  * BFS should visit every square at most once! (check and mark visited during adding to queue)
  * Find count of reachable buildings and if a BFS can't reach all buildings terminate with -1.
  * use a times visited matrix `times[][]` and fill it during BFS 
  * for a `dist[r][c]` to be the best result, `grid[r][c] == 0 && times[r][c] == buildingCount`
  * Visit every square once for each building `O(rcb)` where r:row, c:col, b:building count
  

#### Regular Expression Matching
  * `dp[i][j]` denotes if `s.substring(i)` is matched by `p.substring(j)`
  * if `s.charAt(i-1) == p.charAt(j-1) || p.charAt(j-1) == '.'` then `dp[i][j] = dp[i-1][j-1]
  * if `p.charAt(j-1) == '*'` there are two options
    * it acts as an empty set therefore `dp[i][j] = dp[i-2][j]`
    * it acts as any repetition of same char `dp[i][j] = (s.charAt(i-1) == p.charAt(j-2) || p.charAt(j-2) == '.') && dp[i][j-1]
  * try working out `s:"aaa" p:"a"` and `s:"aaa" p:"a*"`

#### Longest Valid Parentheses
  * `(()()(`
  * first lr then rl
  * keep open/close count and reset count and mark new start when count<0
  * and keep best when count==0;
  

#### Partition Equal Subset Sum
  * question is divide an array of integers to 2 subsets of equal sum
  * find the `sum` of all numbers, must be even (or no two equal subsets)
  * each number is between [1,100] so make a freq map of numbers
  * backtrack with this freqMap to find targetsum of `sum/2`
  * `O(2^n)` n being the length of nums
  * also can be solved by 0/1 knapsack(either we pick a number or not) without repetition(limited count of items),
    * where `boolean dp[i][j]` indicates if sum j can be obtained by first i numbers.
    * so `j` is at most sum and `i` is `nums.length-1`.
    * in that case `dp[i][j] = dp[i-1][j] || dp[i][j-nums[i]]` (either we don't pick the number thus same sum, or we pick the number)
    * when we iterate for every number in `nums` it implicitly works as a freq map(if m number of same element in the array, you can at most use it m times)
    * further space reduction reduces `dp` to a 1d array
    * `O(sum^2)` runtime

#### Lowest Common Ancestor of Deepest Leaves
  * find max depth
  * do a lca with target depth instead of target node

#### Insert Interval
  * three sequential while loops. `O(n)`
  * Insert all intervals that end before newInterval
  * merge all intervals that overlap with newInterval and insert it
  * add all remaining intervals
  * First time I solved it with binary search but no real speed up

#### Check Completeness of a Binary Tree
  * Keep queueing nodes until you poll the first null. `while(q.peek() != null) offerLeft, offerRight`
  * at this point what is left in the queue should be only nulls `while (!q.isEmpty() && q.peek == null) q.poll()`
  * if valid nothing should be in `return q.isEmpty()`
  * For clarity try `[1,null,3] [1,2,null]` cases to see how it works.

#### Insert into circular linked list
  * Attention problem.
  * `O(n)`
  * In a sorted circular list only one element should break the rule of sorted. and one of which is max and the other is min
  * There are 2 cases, insertVal is >= max or <= min this is where you insert
  * other case is simple insertVal is between prev node and next node
  * an edge case is a null node, handle it seperately
  * an edge case is a single node `[3]` or `[3,3,3]`, both of which should be handled by your `do {} while()` loop terminating after a single loop.

#### Missing element in sorted array
  * Naive `O(n)`
  * binary search `O(logn)`
  * care edge case when K is bigger then the actual number of missing numbers, hence initialize with `r=A.length`

#### Is One Edit Distance
  * edge case if they are `equal` return false
  * an other edge case is `S="a" T=""`
  * rest like valid palindrome with 1 live

#### Longest Arithmethic subsequence
  * `dp[index] represents a Map of <difference, maxLength> of all seubsequences ending at index i`;
  * rest is Longest increasing subsequence
    * in order to build upon prev result, diff must be same, so for each new i check each j between [0,i] and increment if it contains a subsequence of diff.
    * maxLength is monotonically increasing per diff, therefore no need to check if prev result was bigger then current.
  * `O(n^2)`

#### Max Consecutive Ones III
  * basically find longest sub array that contains at most K zeroes.
  * keep count of zeroes and two pointers
  * `O(n)`

#### Monotonic Array
  * Two pass, true if you can reach to the end in either one
  
#### Friends of appropriate ages
  * instead of going over 20000 people, count them based on age (120)
  * make a prefix sum array, so you can find the number of people in a range
  * for each age, if there are any people in the age group, find the age range they will befriend
  * `total += count[age] * (sum[maxAge] - sum[minAge]) - count[age]` since people will not befriend themselves

## Max sum of 3 overlapping arrays

#### Closest Binary Search Tree Value
  * top to bottom, 
  * init initial result as root.val
  * update best result at each step
  * if target < node.val and node has left child go left
  * if target > node.val and node has right child go right

#### First Bad Version
  * binary search

#### Minimum remove to make valid parentheses
  * scan lr and keep count and mark `removed[i]`
  * scan rl and do same for opposite parentheses, skip already removed ones

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
* Can build a count array, for things like age. For ex: Instead of going over 20000 people, only go over 120 different ages.
