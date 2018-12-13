# practise checklist

| subject | details |
| ------- | ------- |
| ~~type conversions | long, int, float, double etc ~~|
| type representations | float |
| bit manipulation | arithmethic/logical bit manipulation |
| Knuth-Morris-Pratt | string search in linear time |
| Union find | |

## Problem specific tricks and take-aways

### Basic Calculator III
https://leetcode.com/problems/basic-calculator-iii/submissions/

* Problem 1. When you do recursion, it's easy to pass in starting index but you also need to return the index where you left off, along with the result. Instead of a `String` use a `Queue<Character>` so recursive methods consume a shared queue, eliminating the need for returning an index.
* Problem 2. `0 - 2147483648` You can't parse `Integer.valueOf(2147483648)`. It is bigger then `Integer.MAX_VALUE` therefore build sum with `int sum = 10 * sum + c-'0'`. You will still overflow because Integer.MAX_VALUE is 2147483647. But the sweet thing is `Inteer.MAX_VALUE + 1 = Integer.MIN_VALUE`. and `-Integer.MIN_VALUE = Integer.MIN_VALUE` which is still negative
* Problem 3. `2-1+2`. If you stack numbers (2,1,2) and operators (-,+) then start evaluating, then result is `-1` where as it should have been `3`. The reason is when you stack you evaluate it with wrong precedence `(2-(1+2))`. So what we do instead is keep `sign='+'` and `num=0` initially and build `num` as you read digits. When you encounter a sign, evaluate existing sign & number, and update `sign = last` & `num=0`. Key trick is that + and - operations are stacked as positive and negative numbers but * and / are evaluated immediately with `stack.pop()`
* Problem 4. When you encounter a `(` recur. When you encounter a `)`, break out and return whatever num is in the stack
* Problem 5. offer an extra `queue.offer('+')` initially so that the last `num` and `sign` are also evaluated.

### Minimum swaps to make sequences increasing
https://leetcode.com/problems/minimum-swaps-to-make-sequences-increasing/

* Mighty Dynamic Programming. It is a good example of *we only need the previous step in order to asses next* so `O(1)` memory will do, i.e no need a whole array of states we only need state[i-1]
* One thing should be kept in mind is, the array A and B would always be valid after you do the swap manipulation or not for each element.
* `swap` means for the ith element in A and B, the minimum swaps if we swap A[i] and B[i]
* `noSwap` means for the ith element in A and B, the minimum swaps if we DO NOT swap A[i] and B[i]
* Three cases are as follows, remember we are trying to keep the array valid.

~~~java
        int swap = 1, noSwap = 0;
        for (int i=1; i<A.length; i++) {
            if (A[i-1] >= B[i] || B[i-1] >= A[i]) {
                // 3 5 
                // 2 3 
                // we can only swap A[i], B[i]; if we also swap A[i-1] B[i-1], so swap builds up on prev swap
                // we can only NOT swap A[i], B[i]; if we also NOT swap A[i-1] B[i-1], so noSwap builds up on prev noSwap
                swap = swap + 1;
                // noSwap = noSwap;
                
            } else if (A[i-1] >= A[i] || B[i-1] >= B[i]) {
                // 5 4
                // 3 7
                // we can only swap A[i], B[i]; if we NOT swap A[i-1] B[i-1], so swap builds on prev noSwap
                // we can only NOT swap A[i], B[i]; if we swap A[i-1] B[i-1], so noSwap builds on prev Swap
                int tmp = swap;
                swap = noSwap + 1;
                noSwap = tmp;
            } else {
                // 1 2
                // 1 3
                // either swap or don't swap, the result will still be valid, so aim for minimum
                int min = Math.min(swap,noSwap);
                swap = min + 1;
                noSwap = min;
            }
        }
        
        return Math.min(swap, noSwap);
~~~

### Shortest subarray with sum at least K
https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/

* Make a sum array such that `sum[i] = A[0] + A[1] + .. + A[i-1]`
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

### Min cost to hire k workers
https://leetcode.com/problems/minimum-cost-to-hire-k-workers

* Sort by wage/quality
* Iterate workers from starting from lowest wage/quality,
* add each workers quality to a maxHeap, and keep a sum of current quality
* if total of more than k workers in heap, remove the highest quality worker both from heap and sum
* if k workers in heap, calculate update `ans = Math.min(ans, sum * currentWorker.quality)`
* time complexity `O(nlogn)`

### First missing positive
https://leetcode.com/problems/first-missing-positive

* swap the integer `i`'s that are > 0 and < nums.length in their correct place in nums to index `i-1`.
* go over the array and find the first missing number.
* if no missing number return `nums.length+1`
* in the retrospective everything seems so easy.

### Largest component size by a common factor
https://leetcode.com/problems/largest-component-size-by-common-factor/

* A lot of knowledge about factorization and primes.
* Every integer consists of a combination of prime factors.
* We start by finding primes upto max allowed number (100000). Runtime? `n + n/2 + n/3 + .. 1`
* we link each number to its prime factors. When finding prime factors, key speed up is `if (primes.contains(a)) i=a`. No need to waste iterations when a becomes a prime itself. Oherwise TLE

```java
for (int i=2; i<=a; i++) {
    if (i > a) break;
    if (primes.contains(a)) i = a; // don't waste time if 'a' is (or becomes) a prime
    if (a % i == 0) {
        // .. do stuff
    }
    while (a % i == 0) a /= i;
}
```
* then run a DFS to sink islands and return size of biggest island

### Paint House II
https://leetcode.com/problems/paint-house-ii/

* Assume `dp[i][j]` where i denotes **total cost** since house 0 to i'th house. and j denotes what color we choose. Then the solution would be `O(nk^2)` where n is number of houses and k number of colors.
* Since color is determined by only last house, we don't need a whole array of past values but we just need to keep `bestChoice` for last house, `bestChoice2` for the case our cur house best color selection is the same as prev one, and ofcourse to determine if that is the case keep `colorChoice` of last house so a total of 3 variables. `O(1)` memory complexity. `O(nk)` time complexity

```java
best = 0, best2 = 0, lastColor = -1;
for each house h {
  localBest = Integer.MAX, localBest2 = Integer.MAX, curColor = -1;
  for each color c {
    int totalCost = costs[h][c] + ((c != lastColor) ? best : best2);
    // cost < localBest < localBest2 or, localBest < cost < localBest2
    updateLocalBestsAndColor(totalCost, localBest,localBest2);  
  }
  best = localBest; best2 = localBest2; lastColor = curColor;
}
return best;
```

### Sliding Window Maximum
https://leetcode.com/problems/sliding-window-maximum/

* Can be solved in `O(nlogk)` using a `TreeMap`. but **better** solutions in `O(n)`exist.
* Linear time solution feels similar to Next Greater Element, with a slight modification that is we use `deque` instead of a `stack` since there is a sliding window and we remove elements from the head which would not be possible with a stack. Since every element is queued only once it is amortized `O(n)`

// TODO (There is a dp solution that gets the job done with 2 passes (still `O(n)`)


### Longest substring with at most two distinct characters
https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/

* `O(n)` greedy two pointers and a frequency map.
* increment j until there are more then 2 distinct chars
* increment i until there are 2 distinct chars again.
* a good trick is to use an array and keep count of letters
```java
int[] freq = new int[256];
freq[s.charAt(j)]++;
if (freq[s.charAt(j)] == 1) count++;
...
freq[s.charAt(i)]--;
if (freq[s.charAt(j)] == 0) count--;
```

### Wildcard Matching
https://leetcode.com/problems/wildcard-matching/

* `O(n^2)` with dynamic programming. Idea is to make a `dp = new boolean[plen+1][slen+1]` where `dp[i][j]` shows if `p.substring(i) matches with s.substring(j)`
* `dp[0][0]` is always `1` since two empty strings always match. 
* `dp[0][j]` is always `0` since empty pattern only can match empty string
* `dp[i][0]` is `dp[i-1][0] && p.charAt(i-1) == '*'`
* When pattern has a `*` it can either act as an empty set, or any character therefore `dp[i][j] = dp[i-1][j] || dp[i][j-1]`
* If pattern is not `*` then `dp[i][j] = dp[i-1][j-1] && (p.charAt(i-1) == '?' || p.charAt(i-1) == s.charAt(j-1))` (it must match until now and characters are same

```java
        z a c a b z
      0 1 2 3 4 5 6 
    0 1 0 0 0 0 0 0
 *  1 1 1 1 1 1 1 1
 a  2 0 0 1 0 1 0 0
 ?  3 0 0 0 1 0 1 0
 b  4 0 0 0 0 0 0 0
 *  5 0 0 0 0 0 0 0
```

### Find the Closest Palindrome
https://leetcode.com/submissions/detail/192059344/

* Edge cases full of 999, 1000 and 1001's. 
* Beyond that the heart of problem is to find the root of palindrome. For a `n` like `12345` that is `123`. The closest palindrome should be a palindrome built of either `root`, `root+1` or `root-1`.
* If `n` was already a palindrome then skip `root` because it will produce the duplicate of `n`. 
* Don't forget to remove last element of root when building palindromes of odd length `n`'s. Forex: `n="12345", root="123", palindrome="12321` (last element `3` is removed since odd length)

### Skyline problem
https://leetcode.com/problems/the-skyline-problem/submissions/

* Action only happens when a building starts or ends. Trick is to keep the height of all 'active' buldings in a max priority queue, so at each building start or end we can query the max height in constant time, and if new max height is different then current height we can add the point to the result. 
* Keep in mind there can be same height buildings, but its no problem since a priority queue works with duplicates. 
* Normally `priorityQueue.remove(Object)` takes `O(n)` time which is a time complexity bottle neck for this problem. In order to overcome this, we can lazy remove an object by keeping its occurance frequency in a `toBeRemoved` map and remove during `peek()`

### Alien Dictionary
https://leetcode.com/submissions/detail/191855035/

* Basically a topological sort problem,
* Extract dependency information (which letter comes before the other) by comparing a word to the next word in the dictionary. Check if it has cycles and if not sort them in topological order
* an edge case is to make sure you go through all existing characters in the dictionary and not just the ones that give you dependency information. One easy way of doing that is adding `Arrays.fill(visited,-1)` for all characters and just loop through unvisited `0` ones.

### Course Schedule III
https://leetcode.com/problems/course-schedule-iii/

Trick was to see the greedy approach. `[2,2],[1,3],[1,3],[1,3]`
* Order by tightest deadline comes first
* try fitting them in this order and when it does not fit, use a priority queue to remove the past course with longest duration to make more space.
* All the courses you have added after the long one will still fit (since you managed to add them when you were closer to the deadline)

```java
 int time=0;
 for (int[] course: courses) {
     time += course[0];
     maxTime.offer(course[0]);
     if (time > course[1]) { // exceeds deadline?
         time -= maxTime.poll(); // removes course with longest duration so far
     }
 }
 return maxTime.size();
```

### Word Break II
https://leetcode.com/problems/word-break-ii/

// TODO

### Next Greater element
https://leetcode.com/problems/next-greater-element-i/

* Key observation is given a lsit of decreasing numbers followed by a greater number. Greater number is the next greater element of all previous numbers. `[6,5,3,2,1,4] ==> 4 is NGE of 3,2,1` so use a stack for decreasing numbers and pop them when a greater number arrives. Every number gets stacked at most once so `O(n)`

https://leetcode.com/problems/next-greater-element-ii/

* Use indexes while dealing with an array that contains duplicates. When you store the values you lose their position information. When you store indices, you can access the value by `nums[pos]` anyway and you get to keep position. 

### Maximal Rectangle
https://leetcode.com/problems/maximal-rectangle/

```java
// [0,0,1,0]
// [1,0,1,1]
// [1,0,1,1]
```
for each row, calculate 3 arrays (height, left and right). At each row check prevous row's values(height[c], left[c], right[c]) to calculate new left right boundaries that satisfy accumulated height. 

height is simple: `height[c] = matrix[r][c] == 1 ? height[c] + 1 : 0;`

Trick of finding left is taking the maximum of current rows left boundary and previous rows left boundry

```java
if (matrix[r][c] == 1) left[c] = Math.max(left[c], curLeft) 
else {left[c] = 0 ; curLeft = c+1};
```
Same goes for right edge by iterating from right to left. Since right is naturally greater you need to be taking `Math.min(right[c], curRight)`

At each row calculate the area by `maxArea = Math.max(maxArea, (right-left)*height)` 

``` java
row=0 :
 height = [ 0, 0, 1, 0]
 left   = [ 0, 0, 2, 2]
 right  = [ 4, 4, 3, 4]
 maxArea = 1 // col 2
 
row=1 :
 height = [ 1, 0, 2, 1]
 left   = [ 0, 0, 2, 2]
 right  = [ 1, 4, 3, 4]
 maxArea = 2 // col 2 or 3

row=2 :
 height = [ 2, 0, 3, 2]
 left   = [ 0, 0, 2, 2]
 right  = [ 1, 4, 3, 4]
 maxArea = 4 // col 4
```


## Misc

* `"abc".compareTo("abd")` for string comparison. -1,0,1
* `List<int[]> l = new ArrayList<>()` actually works. Since Java generics support all reference types.
* `List<Integer> list = new ArrayList<>(); int a = list.get(0)` actually works, unless element is null in that case a NPE is thrown.
* `Arrays.sort(A[], Comparator<? extends A>)` is a thing.
* `PriorityQueue<T> p = new PriorityQueue<>(int capacity, new Comparator<T>(){..})` implementation provides `O(log(n))` time for the enqueing and dequeing methods (offer, poll, remove and add); `O(n)` linear time for the remove(Object) and contains(Object) methods; and `O(1)` constant time for the retrieval methods (peek, element, and size).
* maxHeap = minHeap with negative values. Convenient
* A constant time alternative to linear time `remove(Object)` from a `PriorityQueue` is to lazy remove. That is to keep a map of items to be removed instead of removing them and actually `remove()` them when they are retrieved by `poll()` or `peek()`
* `PriorityQueue<T>` does work with duplicates, but a `TreeMap<T,K>` or a `TreeSet<T>` does not. (Map's or Set's  dont have duplicate or null keys)
* A `TreeMap<K,V> map` is a `SortedMap` and `map.entrySet()` will give the entries in the order of their keys. (Given no specific comparator that is the natural ordering of the keys)
* A `Deque<T> t = new LinkedList<>()` is a queue, that can be modified from both head and tail in `O(1)` time. Methods are `offerFirst(), pollFirst(), peekFirst()` and `offerLast(), pollLast(), peekLast()`
* Can also use `ArrayDeque<T>`, dynamically sized Array instead of a linked list. more performance, but does not allow null values. Also can use as a more performant `stack`
* Memoization Big(O) calculation equals to memory consumption of cache.
* substring -> two pointers + map

### Topological Sort
Topological sort is about finding cyclic dependencies and one way to do that is to use multi value `visited[Node]`. 0 is unvisited, 1 is currently visiting, 2 is visited and it has no cyclic dependency. Another objective is to return nodes in a valid topological order. The key here is to stack the node only **after** traversing all its children. 

```java
private boolean topoSort(Node node, int[] visited, Map<Node,Set<Node>> graph, Stack<Node> stack) {
 if (visited[node] == 1) return false; // visiting a parent of self
 if (visited[node] == 2) return true;  // already visited just skip
 
 visited[node] = 1;                    // mark this node as visiting, so we make sure no child comes back here
 for (Node next : graph.get(node)) {
   if (!topoSort(next, visited, graph, stack)) return false;
 }
 visited[node] = 2;                   // exhausted this node
 stack.push(node);                    // SUPER important to do this AFTER traversing children
 return true;
}
```
### Bipartitioning

* **Bi**partitioning  means "There are two groups". Some elements can not be in the same group with the others. Make a graph of elements who can't be with one another. Make it bidirectional. For ex: if `1` can't be with `2` then `2` can't be with `1` as well. That is the key

* Start from one color and dfs your way down painting each neighbor the opposite color. If there is a contradiction return false (you can't partition)

* There could be more then one possibilities. For ex: if `[1,2],[3,4]` can't be together then both `1,4 and 2,3` and `1,3 and 2,4` are valid. That means you can put `3` and `4` to any group you want as long as they are not in the same group. That is why we can get away with starting with color 1 for each unvisited element in the for loop.

~~~java
for (int i=0; i<=N; i++) {
    if (color[i] == 0 && !dfs(graph, color, i, 1)) return false;
}
return true;
...
private boolean dfs(Set<Integer>[] graph, int[] color, int n, int c) {
    color[n] = c;   // 0 unvisited, 1 red, -1 blue
    for (int nei : graph[n]) {
        if (color[nei] == c || (color[nei] == 0 && !dfs(graph, color, nei, -c))) return false;
    }
    return true;
}
~~~

### Substring problems

* Trick is to use two pointers. Make a map of your target letters. and **keep decrementing count even if it is 0**
  * Keep count of letters, resultStart , resultLength
  * iterate tail pointer until you deplete count ( find all letters)
  * mark the solution
  * then start iterating head pointer until solution is not valid again. (count > 0)

Code for *minimum window substring*
```java
int[] map = new int[128];                 // all visible ascii chars (7 LSB of a byte)
for (char c: str.toCharArray()) map[c]++;   // construct map (how many of which letter we need for valid solution)

int count = target.length(), resultLength = Integer.MAX_VALUE, resultStart = 0;
int head=0,tail=0;

while(tail < str.length()) {
  if (map[str.charAt(tail)] > 0) { count--; }     // found a letter we are looking for
  map[str.charAt(tail)]--;                        // decrement even if not a letter we are looking for
  tail++;
  
  while (count==0) {                              // found one, now start incrementing head
    if (tail - head < resultLength) {             // but first update result (we are looking for minimum)
      resultLength = tail-head;
      resultHead = head;                          
    }
    
    if (map[str.charAt(head)] == 0) { count++; }  // we now miss a letter from target back to incrementing tail
    map[str.charAt(head)]++;
    head++;
  }
  
  return resultLength == Integer.MAX_VALUE ? "" : str.substring(resultHead, resultHead + resultLength); 
```

* Imagine a string `S=[abcdebdde]` and target `T=[bde]`. Looking for minimum *subsequence* of S that contains T:
  - Greedy tip. for each starting position in S. The first subsequence you find by a greedy approach will always be the shortest for that starting position.
  - For ex: Possible subsequences of S are `[1,4,5], [1,4,9], [1,7,9], [1,8,9], [5,6,8], [5,7,8]` (by indices)
  - Shortest ones are **[1,4,5]** for starting index 1 and **[5,6,8]** for starting index 5. All other solutions for 1 and 5 must be longer or equal to first solution.
  - Notice how they are also the first ones to find by a naive greedy two pointer scan

* Careful reading questions as there are many different ways of implying rules. Some things to consider:
  - Length? Order? 
    * Find a min length substring of S that contains all characters in T  (Order does not matter, no length restriction)
    * Find a min length substring of S that T is a subsequence of S. (Order matters, no length restriction)
    * Find a substring of S that is a permutation of T. (Order does not matter, length must be same as T)


### Binary Search

Basicaly three variations:

* Variation 1:
~~~java
int left = 0;
int right = length -1;                        // right points to last element
while (left <= right) {                       // left <= right
  int mid = left + (right -left) / 2;
  if (arr[mid] == target) return mid;         // return immediately
  else if (arr[mid] < target)  left = mid +1; // don't include mid in next cycle
  else                        right = mid -1; // don't include mid in next cycle
}
                                              // ends at right + 1 == left
                                              // no more candidates
~~~
~~~java
[1,3,4,6,7,9]   target=7
     v
[1,3,4,6,7,9]   left = 0; right = 5; mid = 2; 4 < 7
         v
[1,3,4,6,7,9]   left = 3; right = 5; mid = 4; 7 == 7 loop ends
~~~

* Variation 2
~~~java
int left = 0;
int right = length;                           // right points to last element + 1;
while (left < right) {                        // left < right
  int mid = left + (right -left) / 2;
  if (arr[mid] < target) left = mid + 1;      // don't include mid in next cycle
  else                  right = mid;          // include mid in next cycle
}
                                              // ends at left == right
                                              // element at left ( or right) is not checked yet
                                              // search space must be at least 2 in size
~~~
~~~java
[1,3,4,6,7,9]   target=7
       v
[1,3,4,6,7,9]   left = 0; right = 6; mid = 3; 6 < 7
         v
[1,3,4,6,7,9]   left = 4; right = 6; mid = 5; 7 !< 7
       v
[1,3,4,6,7,9]   left = 4; right = 5; mid = 4; 6 < 7
         
[1,3,4,6,7,9]   left = 5; right = 5; loop ends
~~~

* Variation 3
~~~java
int left = 0;
int right = length -1;                        // right points to last element
while (left + 1 < right) {                    // left + 1 < right
  int mid = left + (right -left) / 2;
  if (arr[mid] < target) left = mid;          // include mid in next cycle
  else                  right = mid;          // include mid in next cycle
}
                                              // ends at left + 1 == right
                                              // elements at left and right are not checked yet
                                              // search space must be at least 3 in size
~~~
~~~java
[1,3,4,6,7,9]   target=7
     v
[1,3,4,6,7,9]   left = 0; right = 5; mid = 2; 4 < 7
       v
[1,3,4,6,7,9]   left = 2; right = 5; mid = 3; 6 < 7
         v
[1,3,4,6,7,9]   left = 3; right = 5; mid = 4; 7 !< 7
         
[1,3,4,6,7,9]   left = 4; right = 5; loop ends
~~~



## Sorting

### Insertion, Selection, Bubble Sort
Time Complexity of `O(n^2)`
Space Complexity of `O(1)`

### Merge Sort

* Divide and Conquer
* Merge array into halves recursively until 1 element (array of size 1 is always sorted)
* Merge them back with a 2 pointer technique

Time Complexity of `O(nlogn)`
Space complexity of `O(n)`. Because when single threaded and cleanup, max memory consumed is 2n. will create an array of `n + n/2 + n/4..`

### Quicksort

Time Complexity is `O(n^2)` worst case but `O(nlogn)` average. P(worstCase) is very low with random pivot selection strategy.
Space Complexity is `O(n)` worst case (depth of recursion tree) and `O(lgn)` average case (almost in place.. but with recursion tree)

* Divide and Conquer
* Most programming languages use quick sort for library functions.
* Is not a stable sort when pivot selection is random.
* Partition the array by selecting a pivot and swapping elements <= pivot to the left, and > pivot to the right. Return the partitionIndex

~~~java
private int partition(int[] arr, int start, int end) {
  int pivot = arr[end]; // select pivot as last element
  int pIndex = start;
  for (int i=start; i < end; i++) {
    if (arr[i] <= pivot) {
      swap(arr,i,pIndex);
      pIndex++;
    }
  }
  swap(arr,pIndex,end) // finally swap pivot to its right place
  return pIndex
}
~~~

* Call quick sort on two partitions recursively

~~~java
public quickSort(int[] arr, int start, int end) {
  if (start >= end) return; // base case: don't sort single elements and gracefully exit invalid cases
  int pIndex = partition(arr,start,end);
  quickSort(arr,start,pIndex-1);
  quickSort(arr,pIndex+1,end);
~~~

# Heapsort

Time complexity is `O(nlogn)` for worst and average cases.
Space complexity is `O(1)` for worst case. It is an in place sorting algorithm.

* Heaps can be built on dynamic arrays. So heap sort accepts an unsorted input array and builds a max heap over it.
* When the max heap is built, the biggest element will be at arr[0], so we swap it with the last element and remove the biggest element from the heap and repeat same process.

### Backtracking

https://leetcode.com/problems/permutations/discuss/18239/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partioning)

* **Permutations (no distinct check)**
It is basically choose an option, explore, unchoose that option. Complexity is *O(n!)*. If the input have duplicates you end up having duplicates. For input `[a,a,b]` => `[a,a,b], [a,b,a], [a,a,b], [a,b,a], [b,a,a], [b,a,a]`. That is because you have 3 choices at first step (even though only 2 distinct choices) and so on..

~~~java
void permute(result, partial, available) {
  if (available.isEmpty()) {
    result.add(new ArrayList<>(partial)); // mutable object, clone it
    return;
  }

  // choose / explore / unchoose
  for (unit : available) {
    partial.add(unit);
    markUnavailable(unit);
    permute(result, partial, available);
    markAvailable(unit);
    partial.remove(unit);
  }
}
~~~

* **Permutations (distinct from an input that contains duplicates)**
Key is to explore **distinct** choices given an input that contains duplicates. For ex: `[a,b,a]` there should only be 2 distinct choices at first step `a` and `b`. So keep count and get rid of duplicate choices.

~~~java
// permuatations of [a,b,a,b,a]
// available is a map that contains letters as keys, occurance frequency as value. a:3 b:2
// trick is available.keySet() returns either a or b as a valid choice. (Instead of 3 a's and 2 b's)

void permute(result, partial, solutionLength, available) {
  if (partial.size == solutionLength) {
    result.add(new ArrayList<>(partial)); // mutable object, clone it
    return;
  }

  // choose / explore / unchoose
  for (key : available.keySet()) {
    if (available.get(key) > 0) {
      decreaseCountOfKey();
      partial.add(key);
      permute(result, partial, solutionLength, available);
      partial.remove(partial.size() -1);
      increaseCountOfKey();
  }
}
~~~

Here is a tricky alternative approach to get rid of duplicates with a countless used[] array. **Requires sorting** the available choices so that duplciates become consecutive.

`if (used[i] || (i > 0 && nums[i] == nums[i-1] && !used[i-1]) continue;` 
* `used[i]` standard check. number is already used
* `i > 0 && nums[i] == nums[i-1]` it is a duplicate number
* `!used[i-1]` tricky one: assume permutating `[1,2,2,3]`. Talking about `2` at index 2, we could have been here as part of: 
  * `[1->2->2]` in which case previous `2` at index 1 would be a parent as `used[i-1] = true`
  * `[1---->2]` in which case previous `2` at index 1 is not a parent therefore `used[i-1] = false`. Skipped 

~~~java
Arrays.sort(nums); // so that duplicates are consecutive for nums[i] == nums[i-1] check to work

void backtrack(int[] nums, List<Integer> path, List<List<Integer>> result, boolean[] used) {
  for (int i: nums) {
    if (used[i] || (i > 0 && nums[i] == nums[i-1] && !used[i-1]) continue; // skip used ones and duplicates
    path.add(i);
    used[i]=true;
    backtrack(nums,path,result,used);
    used[i]=false;
    path.remove(path.size()-1);
  }
}
~~~

* **Combinations (no distinct check)**

Trick of combinations is that in a single step, in a for loop of choices, is to make your remaining choices smaller and smaller.

For ex: combinations of `[1,2,3]`. Initial step , your for loop should have 3 options as follows: `1` with choices `[2,3]`, `2` with choice `[3]`, and `3` with no further choice `[]`; This is achieved by passing in an index and let your iteration counter `i` starting from it. Also don't forget to set the index parameter for the next recursion based on iteration counter `i`.

Another trick is to add to result at each step of recursion.

~~~java
void combinationsOf(int[] nums, int index, path, result) {
  result.add(new ArrayList<>(path)); // Don't forget to add to result at each level of recursion
  
  for (int i=index; i < nums.length; i++) { // i should start from index
    path.add(i);
    combinationsOf(nums, i+1, path, result); // i+1 is the heart of the algorithm
    path.remove(i);
  }
~~~

* **Combinations (distinct combinations from input with duplicates)**

Pretty much like distinct Permutations alternative approach, less checks to worry about. **Requires sorting** for duplicates to become consecutive.

~~~java
Arrays.sort(nums); // duplicates are now consecutive

void backtrack(int[] nums, int start, path, result) {
  result.add(new ArrayList<>(path)); // make a copy
  
  for (int i=start, i<nums.length; i++) {
    if (i>start && nums[i] == nums[i-1]) continue; // just skip duplicates
    path.add(i);
    backtrack(nums, i+1, path, result);
    path.remove(path.size() -1);
  }
}
~~~


### Arrays

Two sum. Basic technique O(n). 
* In order for two sum to work there should only be one answer for a given `i,j`. For example `arr = [0,0,0,0] sum=0` will fail.

~~~java
Arrays.sort(nums);
int i=0; int j=nums.length-1;
while (i<j) {
  int sum = nums[i]+nums[j];
  if (sum == target)  { addResult([i,j]); i++; j--; }
  else if (sum > target) j--;
  else i++;
}
~~~

Arrays helper methods
~~~java
// Copy an increase capacity of array
Arrays.copyOf(int[] source, int lengthNew)

// from index must be valid, to index can be >= source.length,
// to index is exclusive
Arrays.copyOfRange(int[] source, int from, int to);

// equals for arrays
Arrays.equals(int[] arr1, int[] arr2);

//hashCode for arrays returns int (consistent with Arrays.equals())
Arrays.hashCode(int[] arr)
~~~



### Knuth-Fisher-Yates shuffle
Each position is replaced once (can also replace with itself). Details : https://blog.codinghorror.com/the-danger-of-naivete/
~~~java
Random r = new Random();
for (int i=cards.length -1; i > 0; i--) {
  int n = r.nextInt(i+1);
  swap (cards[i], cards[n]);
}
~~~

### Overflow
```java
(left + right) / 2 = left + (right-left)/2 
```
```java
left * right < target = left < target / right 
```

Left ones are prone to overflow for big left and right values, right ones are safe

```java
Integer.MAX_VALUE = 2^31 - 1 = 2147483647
Integer.MIN_VALUE = -2^31 = -2147483648
```

So when you are converting an Integer to its negative, you should handle the edge case of Integer.MIN_VALUE separately. 
```java
String num = "2147483648"
Integer.valueOf("2147483648") // throws Exception
int sum = 0;
for (char c : num.toCharArray()) sum = 10*sum + c - '0'; // sum = -2147483648
sum = -sum; // intersting tip. sum = -2147483648 (basic calculator III)
```


### String and char tips
*.charAt(int index)* is an easy way to get a single char from string.
  - It returns 'char' which is a primitive type therefore below code will not compile
  ~~~java
  s = "ab";
  if (s.charAt(0) == null)
  ~~~

*.toCharArray()* to make a string mutable

*Character.isDigit(char c)* is char [0..9]

*Character.isLetter(char c)* is char [a..z]&[A..Z] ((white space is not a letter)

*Character.isLetterOrDigit(char c)* is self explanatory.. 

*Character.toLowerCase(char c)*

*string.equalsIgnoreCase(string other)"

### Get digits of a number
~~~java

while (num > 0) {
  int digit = num % 10;
  // stack it (since its going to be in reverse order)
  num = num / 10;
}
~~~
### Mod % and negative numbers
~~~java
 x = -1
 System.out.println(x % 10) // outputs -1
 System.out.println((x+10) % 10) // outputs 9
 
 // x = [0..9] Say you want to decrease x by 3 in mod 10
 (x-3) % 10 // can be negative
 (x+7) % 10 // always positive
~~~

### Encode/Decode integers
Here is a fast hacky way to encode decode multiple int parameters into a single int if you know their ranges
~~~java
// encode (assume every digit can be [0-15])
int code = 16*16*i + 16*j + k;

// decode
int k = code % 16;
code = code / 16;
int j = code % 16;
code = code / 16;
int i = code % 16;
~~~

### Constant time sorting
if you know the range of elements in an array. you can sort it in constant time by mapping counts to indices. for n = [0,50) you need an array of 50 elements You map values to indices and kep count.

```java
int[] exists = new int[50];
for (int i = 0; i < nums.length; i++) {
    exists[nums[i]]++;
}
```

### about pre-order,in-order,post-order
```
  1
 / \
2   3
```

- pre,in,post declares where the root (1) will be
- About children: left always comes before right. 

pre-order = (**1**,2,3)

in-order = (2,**1**,3)

post-order = (2,3,**1**)

### recursive traversal types

**Top-down** means that in each recursion level, visit the node first to come up with some values, and pass these values to its children when calling the function recursively.

**Bottom-up** means that in each recursion level, first call the functions recursively for all the children nodes and then come up with the answer according to the return values and the value of the root node itself.

### binary tree preOrder iterative traverse

```java
while (node != null)
    result.add(node)
    if (node.right != null) stack.push(node.right)
    node = node.left
    if (node == null && !stack.isEmpty())
    node = stack.pop()
```

Process current node, stack the right element if any, and go left. pop in case there is no more left

### binary tree inOrder iterative traverse

```java
while (node != null || !stack.isEmpty())
    while (node != null)
        stack.push(node)
        node = node.left
    node = stack.pop()
    result.add(node)
    node = node.right
```

Go all the way left while stacking current. when there is no more left pop, process value. and go right
repeat until no more in stack and no more right to go.

### binary tree postOrder iterative traverse

pre order = [1,2,3]

post order = [2 3 1]

inverse of post order = [1,3,2] = pre order with left stacked instead of right 

so the trick is to pre order traverse, but with right node first so that the result array in reverse equals post traverse

```java
result is linkedList
while (node != null)
    result.addFirst(node)
    if (node.left != null) stack.push(node.left)
    node = node.right
    if (node == null && !stack.isEmpty())
    node = stack.pop()
```

### Binary Search Tree (BST)

In order traversing a BST will give you all the nodes in ascending order.

in other words, next successor of a given BST node is leftmost element of its right subtree (which is the next node after root in IOT)

To delete an element from a BST,

find the element to be deleted. In order to keep memory usage constant, iterate with previous and current pointers instead of using a stack.

* element has no children: just remove it
* element has one child, swap the child into elements place
* element has two children: 
  1. swap the next successor in elements place.
  2. since successor has no left branch, set successor.left = element.left
  3. if sucessor is not immediate right branch of element, let successors parent.left = successor.right (in a BST a child node.right is never greater than node.val so its always safe to do this trick). Finally set successor.right = elements.right

### Balanced Binary Search Trees

Generally speaking a recursive call back is called to set a parent node to point to a new node after rotation. Something like:

```java
node.left = delete(childNode); // Internally delete does a rotation and returns new root node,
// in other words delete does not concern itself with keeping a reference to its parent.
```

`TreeSet<T> t = new TreeSet<T>(Comparator<? extends T>)` is BST of java (self balanced red black tree). Implementation provides guaranteed `O(logn)` time cost for the basic operations (add, remove and contains). TreeSet does not allow storing duplicates

* `t.higher(E e)` returns the least element in this set strictly > than the given element, or null if no such element.
* `t.ceiling(E e)` returns the least element in this set >= to the given element, or null if no such element.
* `t.lower(E e)`returns the greatest element in this set strictly < the given element, or null if there is no such element.
* `t.floor(E e)` returns the greatest element in this set <= to the given element, or null if no such element.
* `t.tailSet(E fromElement, boolean inclusive)` returns a view of the portion of this set whose elements are greater than (or equal to, if inclusive is true) fromElement.
* `headSet(E toElement, boolean inclusive)` returns a view of the portion of this set whose elements are less than (or equal to, if inclusive is true) toElement.
* `t.subSet(E fromElement, boolean fromInclusive, E toElement, boolean toInclusive)` returns a view of the portion of this set whose elements range from fromElement to toElement.
* `t.first()`, `t.last()`, `t.pollFirst()`, `t.pollLast()` pollFirst and pollLast also remove the elements.

`TreeMap<T> t = new TreeMap<>()` is a map where keys are BST. It also implements the Map interface.

* same methods with TreeSet just concat `Key` or `Entry` as suffix. For ex: `t.lastKey()` or `t.lastEntry()` etc.

### Shortest Path
Trick is to do a BFS. Some points to keep in mind
  * mark nodes 'visited' not when you are getting them from queue, but when you are adding them to queue. This is important in order to not add same node to queue many times over
  * In order to keep queueing next level neighbors while processing current level nodes, trick is to have an inner loop. In other words you should count how many nodes (of same level) are in the queue and iterate by their count. This way when the next round begins you will have processed all current level nodes and queue will be filled with next level nodes.
  
~~~java
int BFS(Node root, Node target) {
    Queue<Node> queue;  // store all nodes which are waiting to be processed
    Set<Node> used;     // store all the used nodes
    int step = 0;       // number of steps neeeded from root to current node
    // initialize
    add root to queue;
    add root to used;
    // BFS
    while (queue is not empty) {
        step = step + 1;
        int size = queue.size(); // looping queue by size for only processing nodes of same level while queueing next level
        for (int i = 0; i < size; ++i) {
            Node cur = the first node in queue;
            return step if cur is target;
            for (Node next : the neighbors of cur) {
                if (next is not in used) {
                    add next to queue;
                    add next to used; // this is important so same node is not added to queue by a neigbor again
                }
            }
            remove the first node from queue;
        }
    }
    return -1; // there is no path from root to target
}
~~~

### Bidirectional search

Bidirectional BFS or DFS search is better when(since) with each step the possible nodes grow exponentially. So imagine there are *b* valid options per node and the distance from start to target is *2d*.  Classic solution then becomes **O(b^2d)** where as if you start traversing from both the start and the end it is **O(b^d + b^d) = O(b^d)** which is far less than **O(b^2d)**. (for b=2 compare 2 * 2^10 vs 2^20. 2^20 has **512 times** more operations)

### Dynamic Programming

01 Matrix Problem (find minimum distance to 0 for each cell) can be solved in O(mn) with only 2 sweeps.
* First sweep, sweep from top-left to bottom-right, compare cells distance to 0's that are above or left of the cell, this will miss 0's that are right or bottom of cell.
* Therefore second sweep, sweep from bottom-right to top left and compare cells that are below or right of the cell. By keeping the minimum of values.

~~~
// initial
0 1 1 1
1 1 1 1
1 1 1 1
1 1 1 0

// after first sweep (top-left to bottom right)
0 1 2 3
1 2 3 4
2 3 4 5
3 4 5 0

// after second sweep (opoosite direction)
0 1 2 3
1 2 3 2
2 3 2 1
3 2 1 0
~~~

* When doing dp if current row only relies on last row for getting values, you usually don't need the whole array but just an `O(1)` space structure is enough.

### Greedy Approach

"Suppose we could come up with the answer in one pass through the input, by simply updating the *'best answer so far'* as we went. What *additional values* would we need to keep updated as we looked at each item in our input, in order to be able to update the 'best answer so far' in constant time?" 
* Max profit in 1 trade: "Best answer so far" is the max difference we can get by the prices we've seen so far, 
* The "additional value" is the minimum price we've seen so far. If we keep that updated, we can use it to calculate the new max profit so far in constant time. The max profit is the larger of previous max profit and current price minus the minimum price seen so far.


### Knuth-Morris-Pratt

Checks is there a prefix that matches the suffix. When string matching fails, we don't have to recheck prefix.

```java
// dp[0] = 0 always

          j i
pattern = a b a b c b c a
dp[len] =[0               ]

// if charAt(i) != charAt(j) then j = dp[j-1] and repeat until j == 0, if j == 0 and chars still don't match
// then i++ move on;

          j   i
pattern = a b a b c b c a
dp[len] =[0 0             ]

// if charAt(i) == charAt(j) then dp[i] = j + 1 and i++; j++;

            j   i
pattern = a b a b c b c a
dp[len] =[0 0 1           ]

            j           i
pattern = a b a b c b c a
dp[len] =[0 0 1 2 0 0 0 1 ]

```
