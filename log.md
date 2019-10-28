## TODO
  * 21.11.2019
    - permutations I,II, subsets I,II
  * 20.11.2019 
    - bst iterative, bfs recursive
  * 07.11.2019
    - permutations I,II, subsets I,II
  * 06.11.2019 
    - bst iterative, bfs recursive
  * 31.10.2019
    - permutations I,II, subsets I,II
  * 30.10.2019 
    - bst iterative, bfs recursive

## 28.10.2019
  * Amazon explore strings and arrays
    * TwoSum, (map of indices + greedy)
    * Longest substring without repeating chars (set + 2 pointers)
    * string atoi  (overflow, null pointer check, if else),
    * container with most water (greedy, two pointer what if `height[l] == height[r]`? in that case  either `l++` or `r--` would not be an optimal solution since `height==Math.min(h[l],[h[r])` will stay the same and `r-l` will be one less.)
    
## 27.10.2019
  * ~~bst iterative, bfs recursive (skipped)~~
  * symetric tree
    * simultaneous traversing, recursive: two nodes as parameters, iterative: offer two nodes into queue
  * univalue tree
  * permutations I,II, subsets I,II
  
## 26.10.2019
  * binary tree pre, in, post order traversals. both recursive, and iterative
    * iterative post order traversal = inverse of right child first pre order traversal
    * need `LinkedList<>` reference instead of `List<>` to use `addFirst()`
  * BFS, iterative and recursive
    * recursive uses parameter `int level`
