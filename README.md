# practise checklist

| subject | details |
| ------- | ------- |
| binary search | Study edge cases |
| type conversions | long, int, float, double etc |
| type representations | float |
| bit manipulation | arithmethic/logical bit manipulation |
| priority queues | |
| heap | |
|sorting algorithms| |

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
  for (int i=0; i < end; i++) {
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

* **Permutations (distinct)**
It is basically choose an option, explore, unchoose that option. Complexity is a minimum of *O(n!)*

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

* **Permutations (non-distinct)**
Key is to explore **distinct** choices. Instead of all avalable.
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

Here is an alternative approach:

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

* **Combinations (distinct)**

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

* **Combinations (non-distinct)**

Pretty much like Permutations (non-distinct), less checks to worry about.

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

Two sum. Basic technique O(n)

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
Arrays.copyOfRange(int[] source, int from, int to);
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
Integer.MAX_VALUE = -2^31 = 2147483647
Integer.MIN_VALUE = 2^31 - 1 = -2147483648
```

So when you are converting an Integer to its negative, you should handle the edge case of Integer.MIN_VALUE separately

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


