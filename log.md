## TODO
  * Later
    * knuth-moris-pratt
  * 21.11.2019
    - permutations I,II, subsets I,II
  * 20.11.2019 
    - bst iterative, bfs recursive
  * 07.11.2019
    - permutations I,II, subsets I,II
  * 06.11.2019 
    - bst iterative, bfs recursive
  * 03.11.2019
    - reverse a linked list iterative, recursive
  * 02.11.2019
    - Minimum window substring related problems
    - Trapping rain water O(1) space 
    
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
    
## 27.10.2019~~
  * symetric tree
    * simultaneous traversing, recursive: two nodes as parameters, iterative: offer two nodes into queue
  * univalue tree
  * permutations I,II, subsets I,II (permutations use used[], when duplicates always `Arrays.sort`)
  
## 26.10.2019
  * binary tree pre, in, post order traversals. both recursive, and iterative
    * iterative post order traversal = inverse of right child first pre order traversal
    * need `LinkedList<>` reference instead of `List<>` to use `addFirst()`
  * BFS, iterative and recursive
    * recursive uses parameter `int level`
