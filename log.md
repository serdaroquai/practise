## TODO
  * Later
    * knuth-moris-pratt
    * A* search (*cut off trees*)
    * Hadlocks Algorithm (*cut of trees*)
    * Tarjans, (find nodes that disjoin the graph)  (*critical connection*)
    
  * 21.11.2019
    - permutations I,II, subsets I,II
  * 20.11.2019 
    - bst iterative, bfs recursive
  * 07.11.2019
    - permutations I,II, subsets I,II
  * 04.11.2019
    - Minimum window substring related problems

## 14.11.2019
  * Amazon Explore
    * Course Schedule (bfs for each target `O(mn*mn)` *worst case mn targets, trick is to set visited preemptively while checking valid for algorithm to barely make it in time. There are better alternatives)
    * Flood fill
## 12.11.2019
  * Amazon Explore
    * Course schedule (topological sort)
    * lowest common ancestor
    * diameter of binary tree (easier version of binary tree max path sum)
    * cut of trees for golf event
## 10.11.2019
  * recall 
    * Trapping rain water O(1) space
  * Amazon Explore
    * merge K sorted lists (merging by pairs is `O(nk)`, priorityqueue is `O(nlogk)`, k: length of list, n: total number of elements)
    * validate binary search tree (iterative or recursive)
    * binary tree zigzag (use a linkedlist row, each time you add a value addFirst or add based on level);
    * binary tree max path sum (at each node best result is either `node`, `left +node`, `right + node`, or `left + right + node`. what we return is `Math.max(left+node,right+node)`)
    * word ladder ( bfs start from both ends)
    
  
## 08.11.2019
  * critical connection (needs a detailed explanation, Tarjans algorithm)
## 07.11.2019
  * binary-tree-coloring-game
  * critical connection (Failed this popular amazon question)
## 06.11.2019
  * Subtree of another tree
  * bfs iterative, bfs recursive
  * Amazon Online assessment questions
    * top K competitors (construct map<competitors,count>. quickselect competitors array for top K elements based on count)
    * zombie matrix, rotten tomatoes (multi source bfs, keep count of fresh oranges left in order to deal with edge cases)

## 05.11.2019
  * Amazon explore binary search
    * find minimum in sorted array(binary search `r=m`)
    * find first and last posiiton of element in sorted array(attention, binary search both `r=m` and `l=m`, return conditions are tricky)
    * find K closest elements (**★** try binary searching a k sized array based on left or right end being farther)
    * find peak element
## 04.11.2019
  * Amazon explore binary search
    * Binary Search template 1
    * Sqrt(x) (attention, divison by zero, multiplication overflow, `(m <= target /m) ans = m`)
    * Guess number higher or lower
    * Search in rotated sorted array (binary search, if left is sorted and value is in between go left else right, if vice versa..)
    * First Bad Version (binary search `r=m`)
    * Find Peak Element (binary search `r=m`)
## 03.11.2019
  * reverse a linked list iterative, recursive
    * KClosestPointsToOrigin (Quickselect `O(n)`)
    
## 02.11.2019
  * Amazon OA 2019
    * Find pair with given Sum (Two sum with a return max condition)
    * Merge Two Sorted Lists
    * Sub tree of another tree ( attention. `O(s*t)`, you need **two** methods, one checking for exact match and one calling that for each node, trying to combine the two under the original question fails)
    * Search a 2D matrix || ( select bottom left or top right, compare with target r- or c+)
  * Search a 2D matrix (1D binary search encoding based on col length)
    
## 01.11.2019
  * Amazon OA 2019
    * Optimal Utilization (sort, 2 pointers, care for possible same elements)
    * Reorder Logs
    * Minimum Cost to connect sticks (priorityQueue is  min heap)
    * Treasure Island (BFS, shortest path, count steps)
    * Treasure Island II (Multiple Source BFS, visited can be same since if one source reach there first, then the others can't be optimal solution anyway)
    * 01 Matrix (for all non 0's, sweep rightbottom `matrix[r][c] = Math.min(top,left)+1`, sweep leftTop `matrix[r][c] = Math.min(matrix[r][c], Math.min(bottom,right)+1)`, DP)

## 31.10.2019
  * amazon explorer linked lists
    * reverse a linked list iterative, recursive (`recursive(self, parent)` topdown, iterative: use `cur,prev` return `prev`)
    * reverse nodes in k groups (**hard** focus, lots of tricks)
    * copy list with random pointer ( hashmap O(n) space, no hashmap: create new nodes and insert them between respective originals)
  * goback
    * permutations I,II, subsets I,II
## 30.10.2019 
  *  go back
    * bst iterative, bfs recursive
  * amazon explorer linked lists
    * add two numbers
    * merge two sorted lists
    
## 29.10.2019
  * Amazon explore strings and arrays
    * Group anagrams (encode char freq as string, use it as group key)
    * **Minimum window substring** (freqmap, count, two pointers)
    * Compare versions (attention problem, parse, compare, Tuple)
    * Product of Array except self (use multiplication arrays from left2right, right2left)
    * Missing number (sum of n numbers - sum of actual elements)
    * Integer to English Words ( % 1000, <20 <100 <1000 helper recursive, zero edge case, **num % 1000 == 0 edge case, dont append million, billion etc..**)
    * First Unique Character In a String
    * Valid Parantheses (use stack, count only does **not** work, `([)]`)
    * Most Common Word ( `.split("\\W+")` split by non word character(s))
    * Reorder Data in Log Files (one big comparator, to keep natural order of array in a comparator return 0)
    * Trapping rain water (two pointer, l2r r2l max height arrays - block height, can be improved to `O(1)` space)
  * Number of islands (dfs and sink)
    
## 28.10.2019
  * Amazon explore strings and arrays
    * TwoSum, (map of indices + greedy)
    * Longest substring without repeating chars (set + 2 pointers)
    * string atoi  (attention problem, overflow, null pointer check, if else),
    * container with most water (greedy, two pointer what if `height[l] == height[r]`? in that case  either `l++` or `r--` would not be an optimal solution since `height==Math.min(h[l],[h[r])` will stay the same and `r-l` will be one less.)
    * integer to roman (recursive,int % and modulo)
    * roman to integer (easier, just parsing, `IV IX` etc, first add then substract)
    * 3sum, 3sum closest( sort array, `n * 2sum` get rid of duplicates by incrementing in a while loop)
    * strstr aka: indexOf ( **knuth morris pratt** `O(m+n)`)
    * rotate image (in place rotations, outer to inner circles)
    
## 27.10.2019
  * symetric tree
    * simultaneous traversing, recursive: two nodes as parameters, iterative: offer two nodes into queue
  * univalue tree
  * permutations I,II, subsets I,II (permutations use used[], when duplicates always `Arrays.sort`)
  
## 26.10.2019
  * binary tree pre, in, post order traversals. both recursive, and iterative
    * iterative post order traversal = inverse of right child first in order traversal
    * need `LinkedList<>` reference instead of `List<>` to use `addFirst()`
  * BFS, iterative and recursive
    * recursive uses parameter `int level`
