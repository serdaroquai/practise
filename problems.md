## Problem specific tricks and take-aways

### Expression Add Operators
https://leetcode.com/problems/expression-add-operators/submissions/

* Tricks: 
	* keep last term as parameter so we can roll back multiplication.
    * send `-cur` for subtraction and `prev*cur` for multiplication so that we get away without any if blocks  `helper(s, i+1, target, eval-prev + prev*cur, prev*cur, sb, result);` for multiplication
	* keep `int len = sb.length()` so easier to back track by `sb.setLength(len)`
	* append operators first
	
* Edge cases,
  * use `long` to avoid overflow
	* numbers startig with `0` should stopped. handle this first so we don't accidently skip it for `pos=0` case.
	* case `pos=0` should be handled separately.
	  * we don't want to stat by appending operators and we can't just trim it because stating with 0 creates many duplications `0-1, 0+1, 0*1`
		
* Runtime `O(3^(n-1))`, Space `O(n)` (recursion tree depth)

```java
public List<String> addOperators(String num, int target) {
		List<String> result = new ArrayList<>();
		if (num == null || num.length() == 0) return result;

		helper(num.toCharArray(), 0, (long) target, 0, 0, new StringBuilder(), result);

		return result;
}

private void helper(char[] s, int pos, long target, long eval, long prev, StringBuilder sb, List<String> result) {
		if (pos == s.length) {
				if (eval == target) result.add(sb.toString());
				return;
		}

		long cur = 0;
		for (int i=pos; i<s.length; i++) {
				if (i != pos && cur == 0) break; // kill the numbers starting with 0 first check
				cur = cur*10 + s[i] - '0';
				if (cur > Integer.MAX_VALUE) break; // overflow

				int len = sb.length();

				if (pos == 0) { // special treatment for start
						sb.append(cur);
						helper(s, i+1, target, cur, cur, sb, result);
						sb.setLength(len);
						continue; // don't breaki continue 
				}

				sb.append('+').append(cur);
				helper(s, i+1, target, eval+cur, cur, sb, result);
				sb.setLength(len);

				sb.append('-').append(cur);
				helper(s, i+1, target, eval-cur, -cur, sb, result); // notice -cur 
				sb.setLength(len);

				sb.append('*').append(cur);;
				helper(s, i+1, target, eval-prev + prev*cur, prev*cur, sb, result); // notice prev*cur
				sb.setLength(len);

		}
}
```

### Convert BST to Doubly Linked List
https://leetcode.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list

* Iterative in order traverse while keeping a prev node.
* Introduce a dummy prev so that we don't need to null check
  * At the end of traversing, prev will be last node, and dummy.right will be start node, helps tying them circular
```java
public Node treeToDoublyList(Node cur) {
	if (cur == null) return cur;

	Node dummy = new Node(-1);
	Node prev = dummy;

	Stack<Node> stack = new Stack<>();
	while (cur!=null || !stack.isEmpty()) {
		while (cur != null) {
			stack.push(cur);
			cur = cur.left;
		}
		cur = stack.pop();
		prev.right = cur;
		cur.left = prev;
		prev = cur;
		cur = cur.right;
	}

	dummy = dummy.right;
	dummy.left = prev;
	prev.right = dummy;

	return dummy;
}
```
	
### Integer to English Words
https://leetcode.com/problems/integer-to-english-words

* Dealing with it in `%1000` is the easiest as thousands have a repetitive pattern.
  * Trick is to have a recursive helper function that deals with `num<20`, `num<100` and `num<1000`. Use recursion
```java
private String helper(int num) {
  if (num < 20) 
    return lessThan20[num];
  else if (num < 100)
    return tens[num / 10 % 10] + helper(num % 10);
  else 
    return lessThan20[num / 100] + " Hundred" + helper(num % 100); 
}
```
  * In the main loop use a stack to reorder each part.
    * stack `thousands[step]` **before** word and only if `num % 1000 != 0` (first word has no suffix)
    * increment step anyway	
```java
  int step = 0;
  Stack<String> stack = new Stack<>();
  while (num > 0) {
    if (num % 1000 != 0) stack.push(thousands[step]);
    stack.push(helper(num % 1000));

    num /= 1000;
    step++;
  }
```
  * Easier to handle `Zero` separately as the only edge case.
  * Use white spaces **before** literals is easier, just trim the first one before returning result.
	* Some test cases to think of `0`, `1000000`, `12345`, `50123`
	
### Longest Valid Parenthesis
https://leetcode.com/problems/longest-valid-parentheses/

* O(n) time O(n) memory stack solution
  * Insert initial -1 to stack. 
  * for each '(' stack index.
  * For each ')' pop the stack.
    * If stack is empty previous string is invalid, push(i)
    * if not `best = Math.max(best, i-stack.peek())`

* O(n) time O(1) memory 2 pass solution
  * sweep arr left to right, for each '(' openCount++ and for each ')' closeCount++
    * if closeCount > openCount, invalid parentheses reset counts to 0
    * if openCount = closeCount `best = Math.max(best, openCount + closeCount)`
  * sweep arr right to left and repeat same proces mirrored.
  
### Add and Search words
https://leetcode.com/problems/add-and-search-word-data-structure-design/

* Typical Trie problem.
* Trie have `TrieNode[] children` and `boolean isWord`
* for searching `.` (can replace any char) there is no trick, just do an exhaustive search. 
* It is a good reminder to think about how not to get stuck trying to find a better solution that does not exist.
* Time complexity
  * addWord() is `O(n)` n = length of the new word
  * search(s) is `O(c)` number of chars in whole trie. (Worst case is we have all the words `aaa, aab, aac... zzz` and we are searching `....` so we will go through all combinations. also equals 26^n n being length
* Space complexity `O(c)` c = number of chars in whole trie.

### LRU cache
https://leetcode.com/problems/lru-cache

* `ListNode` => key, value, next, prev
* `Map<Integer, ListNode> map` and a `DoublyLinkedList list` implementation.
* Doubly Linked list only needs two methods. `ListNode removeFirst()` for eviction and `addLast(ListNode n)` for most recently used update.
* A good trick is to use dummy nodes for start and end to get rid of most null edge cases.

```java
class DoublyLinkedList{
        
	ListNode start, end;

	public DoublyLinkedList() {
	    start = new ListNode(-1,-1);
	    end = new ListNode(-1,-1);
	    start.next = end;
	    end.prev = start;
	}

	void addLast(ListNode n) {
	    //pluck n if it is an existing node
	    if (n.prev != null) n.prev.next = n.next;
	    if (n.next != null) n.next.prev = n.prev;

	    // bind to prev node
	    n.prev = end.prev;
	    end.prev.next = n;

	    // bind to end node
	    n.next = end;
	    end.prev = n;
	}

	ListNode removeFirst() {
	    ListNode result = start.next;
	    
	    result.prev.next = result.next;
	    result.next.prev = result.prev;
	    
	    return result;
	}
}
```

### Serailize and Deserialize Binary Tree (follow up: BST)
https://leetcode.com/problems/serialize-and-deserialize-binary-tree/

* Serailize: Iterative pre-order(actually any order) with "X" as line breakers.
* Deserialize: 
  - Use the same order, 
  - check null case and handle root node separately (keep reference since you will return it)
  - for each node in queue, add left and right nodes if not marked null (and queue them)

```java
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            TreeNode cur = queue.poll();
            if (cur == null) sb.append("X");
            else {
                sb.append(cur.val);
                queue.offer(cur.left);
                queue.offer(cur.right);
            }
            sb.append(",");
        }
        sb.setLength(sb.length()-1); // remove last splitter
        return sb.toString();
    }

    public TreeNode deserialize(String data) {
        String[] tokens = data.split(",");
        if (tokens[0].equals("X")) return null;
        
        TreeNode head = new TreeNode(Integer.valueOf(tokens[0]));
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(head);
        
        int i = 0;                  
        while (i<tokens.length-1) {
            TreeNode parent = queue.poll();
            
            if (!tokens[++i].equals("X")) {
                parent.left = new TreeNode(Integer.valueOf(tokens[i]));
                queue.offer(parent.left);
            }
            if (!tokens[++i].equals("X")) {
                parent.right = new TreeNode(Integer.valueOf(tokens[i]));
                queue.offer(parent.right);
            }
        }
        
        return head;
    }
```


**Follow up:**
https://leetcode.com/problems/serialize-and-deserialize-bst/

What if the tree was BST? Can we make the encoded string more compact? The whole point of this question is to use the BST property to at get rid of nulls in your serialized string. Anything less is a suboptimal solution.

  - To Serialize just do an iterative pre-order traversal. (in real world, recursive can stack overflow if BST is unbalanced)
  - To Deserialize:
	  - if next node is smaller then parent just (left) add to parent.
	  - if next node is larger then parent pop the stack until you find the largest parent you can (right) add to.
	  - finally stack the next node as a candidate parent.

Each node gets in and out of stack only once, so time complexity `O(n)`, and the serialized string has no nulls, or markers only the nodes.

```java
    public String serialize(TreeNode n) {
        if (n == null) return "";
        StringBuilder sb = new StringBuilder();
        
        Stack<TreeNode> stack = new Stack<>();
        while (n!=null) {
            sb.append(n.val).append(",");
            if (n.right != null) stack.push(n.right);
            n = (n.left == null && !stack.isEmpty()) ? stack.pop() : n.left;
        }
        sb.setLength(sb.length()-1);
        return sb.toString();
    }

    public TreeNode deserialize(String s) {
        if ("".equals(s)) return null;
        String[] tokens = s.split(",");
        
        Stack<TreeNode> stack = new Stack<>();
        TreeNode head = new TreeNode(Integer.valueOf(tokens[0]));
        stack.push(head);
        
        for (int i=1;i<tokens.length;i++) {
            
            TreeNode parent = stack.peek();
            TreeNode next = new TreeNode(Integer.valueOf(tokens[i]));
            
            if (next.val < parent.val) parent.left = next;
            else {
                parent = stack.pop();
                while (!stack.isEmpty() && next.val > stack.peek().val) parent = stack.pop();
                parent.right = next;
            }
            stack.push(next);
        }
        return head;
    }
```



### Remove Invalid Parentheses
https://leetcode.com/problems/remove-invalid-parentheses/

* Naive solution BFS
  - check if string is valid, if yes add to result then found = true (so we stop adding new string to queue)
  - for each '(' or ')' in string en-queue a substring removing it.
  - we need a visited set to get rid of duplicates for ex since removing 1 or 2 from `())` would result in same sequence.
* We can get faster if we get rid of duplicates in the first place
  - (**))** ---> () so only remove the first one of )). (consecutive removals of same type yield same result)
  - **(**()**(**() ---> ()() remove 0 then 3, or 3 then 0 yields same result. So keep last removal index and only remove > last.
* At this point we don't need a visited set, we can get even faster by some optimizations
  - So not remove a pair of valid parentheses. If we remove a '(', there is no point removing a ')' next. Best case we would still be valid eventually but we would waste 2 operations and miss optimal solution anyway.
  
```java
class Tuple {
    String string;
    int start;
    char removed;
}
public List<String> removeInvalidParentheses(String s) {

    List<String> result = new ArrayList<>();

    Queue<Tuple> q = new LinkedList<>();
    q.offer(new Tuple(s,0,')'));

    while (!q.isEmpty()) {
        int size = q.size();
        for (int i=0; i<size; i++) {

            Tuple t = q.poll();
            if (isValid(t.string)) result.add(t.string);
            if (!result.isEmpty()) continue;

            for (int j=t.start; j<t.string.length(); j++) { //start from last position (bullet 2)

                char ch = t.string.charAt(j);

                if (ch != '(' && ch != ')') continue; // skip other letters
                if (j != t.start && ch == t.string.charAt(j-1)) continue; // skip same consecutive removals (bullet 1)
                if (t.removed == '(' && ch == ')') continue; // don't remove valid parentheses (bullet 3)

                q.offer(new Tuple(t.string.substring(0,j) + t.string.substring(j+1), j, ch));

            }    
        }
    }
    return result;
}
```



### Delete Columns to Make Sorted III
https://leetcode.com/problems/delete-columns-to-make-sorted-iii/

* this is a *longest increasing subsequence* problem.
* Instead of finding the LIS for a single word, we do it simulatenously for all words in the array
* Time `O(m*n^2)` m : length of word array, n length of word.

### Basic Calculator III
https://leetcode.com/problems/basic-calculator-iii/submissions/

* Problem 1. When you do recursion, it's easy to pass in starting index but you also need to return the index where you left off, along with the result. Instead of a `String` use a `Queue<Character>` so recursive methods consume a shared queue, eliminating the need for returning an index.
* Problem 2. `0 - 2147483648` You can't parse `Integer.valueOf(2147483648)`. It is bigger then `Integer.MAX_VALUE` therefore build sum with `int sum = 10 * sum + c-'0'`. You will still overflow because Integer.MAX_VALUE is 2147483647. But the sweet thing is `Inteer.MAX_VALUE + 1 = Integer.MIN_VALUE`. and `-Integer.MIN_VALUE = Integer.MIN_VALUE` which is still negative
* Problem 3. `2-1+2`. If you stack numbers (2,1,2) and operators (-,+) then start evaluating, then result is `-1` where as it should have been `3`. The reason is when you stack you evaluate it with wrong precedence `(2-(1+2))`. So what we do instead is keep `sign='+'` and `num=0` initially and build `num` as you read digits. When you encounter a sign, evaluate existing sign & number, and update `sign = last` & `num=0`. Key trick is that + and - operations are stacked as positive and negative numbers but * and / are evaluated immediately with `stack.pop()`
* Problem 4. When you encounter a `(` recur. When you encounter a `)`, break out and return whatever num is in the stack
* Problem 5. offer an extra `queue.offer(')')` initially so that the last `num` and `sign` are also evaluated and the result is stacked. When the last result is stacked, stack should contain a bunch of positive and negative numbers. build the sume and return it.

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
* What makes this problem hard (and not solved by two pointers is negative numbers)
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
### Maximum Width Ramp
https://leetcode.com/problems/maximum-width-ramp/

* Two tricks:
  - First Trick: Assume first few elements are 2,3 . When evaluating the next element (assume it is 4), since we are looking for the maximum width of indexes j - i, if next element(4) is greater then 3, it will automatically be greater then 2. And since 2 has a smaller index that only makes sense to:
  - iterate elements from 0 to A.length and stack the decreasing sequence. (skip the ones that are increasing) #next_greater_element
  - Second Trick: Now when we are actually finding the max width indexes, it makes sense to iterate right to left starting from A.length-1 to 0 so that we can pop elements from stack making sure this is the longest possible answer.
  - take [3,2,1,2,2] for example. Evaluating right most 2, stack should be [3,2,1] and we can pop 1 and 2 forever to get the result (j-i = 4 - 1 = 3, since element 2 at index 3 would only yield 3 - 1 = 2 (which is always shorter).

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
