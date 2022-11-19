---
title: "📝 Data structures and algorithms"
date: 2022-10-27
searchHidden: true
TocOpen: true
robotsNoIndex: true
---

Some notes about DSA

<!--more-->

### Links

- [Cracking the coding interview](https://www.amazon.com/Cracking-Coding-Interview-Programming-Questions/dp/0984782850)
- [Neetcode](https://neetcode.io/practice)
- [Leetcode](https://leetcode.com/)
- [Project Euler](https://projecteuler.net/)
- [HackerRank](https://www.hackerrank.com/)

### Big O

- [Neetcode](https://youtu.be/BgLTDT03QtU)
- [Big O Cheat Sheet](https://www.bigocheatsheet.com/)
- Recursive with multiple branches: O(branches^depth), if not repeated work, O(n).
- Fibonacci: O(2^n), with memoization: O(n)
- Permutations, TSP: O(n!)
- Binary search: O(log n) (divide search space)

## Arrays & Hashing

---

- 👀[**Two Sum**](https://leetcode.com/problems/two-sum/):
  [💡](https://youtu.be/KLlXCFG5TnA)
  - `nums = [2,7,11,15], target = 9` => `[0,1]`
  - HashMap `{n: i}`
  - O(n) time, O(n) space
- 👀[**Contains Duplicate**](https://leetcode.com/problems/contains-duplicate/):
  [💡](https://www.youtube.com/watch?v=3OamzN90kPg)
  Naive, Sort, Set
- 👀[**Valid Anagram**](https://leetcode.com/problems/valid-anagram/):
  [💡](https://www.youtube.com/watch?v=9UtInBqnCgA)
  Two Hashmap (or 1 and substract), Sort
- 👀[**Group Anagrams**](https://leetcode.com/problems/group-anagrams/):
  [💡](https://www.youtube.com/watch?v=vzdNOK2oB2E)
  Hashmap (sorted counter as key)
- 👀[**Top K Frequent Elements**](https://leetcode.com/problems/top-k-frequent-elements/):
  [💡](https://www.youtube.com/watch?v=YPTqKIgVk-k)
  Count, Group by frequency (list of lists)
- 👀[**Product of Array Except Self**](https://leetcode.com/problems/product-of-array-except-self/):
  [💡](https://www.youtube.com/watch?v=bNvIQI2wAjk)
  Prefix product, Suffix product.
- 👀[**Encode and Decode Strings**](https://www.lintcode.com/problem/659/):
  [💡](https://www.youtube.com/watch?v=B1k_sxOSgv8)
  Length + Separator
- 👀[**Longest Consecutive Sequence**](https://leetcode.com/problems/longest-consecutive-sequence/):
  [💡](https://www.youtube.com/watch?v=P6RZZMu_maU)
  Set and start if n-1 not in set

---

- ✅[**Minimum Time Difference**](https://leetcode.com/problems/minimum-time-difference/):
  [💡](https://leetcode.com/problems/minimum-time-difference/solutions/100637/python-straightforward-with-explanation/comments/104667)
  - `timePoints = ["23:59","00:00"]` => `1`
  - map to minutes & sort, `(time[i]-time[i-1])%(24*60)`
  - O(nlogn) time, O(n) space
- ✅[**Logger Rate Limiter**](https://gist.github.com/kuntalchandra/7822e388e0d2d78ec27f566266584b49):
  [💡](https://gist.github.com/kuntalchandra/7822e388e0d2d78ec27f566266584b49)
  - `shouldPrintMessage(1, "foo"), shouldPrintMessage(3, "foo")` => `true, false`
  - Hashmap `{message: timestamp}`, `if self.cache[message] + 10 > timestamp`
  - O(1) time, O(n) space
- ✅[**Number of matching subsequence**](https://leetcode.com/problems/number-of-matching-subsequences/):
  [💡](https://leetcode.com/problems/number-of-matching-subsequences/solutions/117634/efficient-and-simple-go-through-words-in-parallel-with-explanation/)
  - `s = "abcde", words = ["a","bb","acd","ace"]` => `3`
  - Hashmap `{char: [(word_idx, current_idx)]}`
  - O(s.length+sumcharwords) time, O(#words) space
- ✅[**Text Justification**](https://leetcode.com/problems/text-justification/):
  [💡](https://leetcode.com/problems/text-justification/solutions/24891/concise-python-solution-10-lines/?orderBy=most_votes)
  - `words = ["This", "is", "an", "example", "of", "text", "justification."] w = 16` => `["This is an", "example of text", "justification. "]`
  - `if num_of_letters + len(w) + len(cur) > maxWidth:`, round robin
- ✅[**Find original array from doubled array**](https://leetcode.com/problems/find-original-array-from-doubled-array/):
  [💡](https://leetcode.com/problems/find-original-array-from-doubled-array/solutions/1470959/java-c-python-match-from-the-smallest-or-biggest-100/)
  - `changed = [1,3,4,2,6,8]` => `[1,3,4]`
  - iterate sorted count, `if count[x*2] >= count[x]`, handle 0, `cnt[2 * x] -= cnt[x]`
  - O(n + mlogm) time, O(m) space (m = unique elements)
- [**Valid Sudoku**](https://leetcode.com/problems/valid-sudoku/):
  [💡](https://www.youtube.com/watch?v=TjFXEUCMqI8)
  `squares[(r // 3, c // 3)].add(board[r][c])`
- [**Close strings**](https://leetcode.com/problems/determine-if-two-strings-are-close):
  [💡](https://leetcode.com/problems/determine-if-two-strings-are-close/solutions/1029064/python-oneliner-with-counter-explained/?orderBy=most_votes)
`set(w1) == set(w2) and Counter(Counter(w1).values()) == Counter(Counter(w2).values()`

## Two Pointers

---

- 👀[**Valid Palindrome**](https://leetcode.com/problems/valid-palindrome/):
  [💡](https://www.youtube.com/watch?v=jJXJ16kPFWg)
  2 pointers
- 👀[**3Sum**](https://leetcode.com/problems/3sum/):
  [💡](https://www.youtube.com/watch?v=jzZsG8n2R9A)
  sort, 2 pointers
- 👀[**Container With Most Water**](https://leetcode.com/problems/container-with-most-water/):
  [💡](https://www.youtube.com/watch?v=UuiTKBwPgAo)
  start from both ends, move the smaller one

---

- [**Two Sum II**](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/):
  2 pointers.
- [**Trapping Rain Water**](https://leetcode.com/problems/trapping-rain-water/):
  DP, can optimize to O(1) space by using 2 pointers
- [**String Compression**](https://leetcode.com/problems/string-compression/):
  2 pointers, slow and fast.

## Sliding Window

---

- 👀[**Best Time to Buy and Sell Stock**](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/):
  [💡](https://www.youtube.com/watch?v=1pkOgXD63yU)
  [Kadane's algorithm](https://raw.githubusercontent.com/neetcode-gh/leetcode/main/python/121-Best-Time-To-Buy-and-Sell-Stock.py)
- 👀[**Longest Substring Without Repeating Characters**](https://leetcode.com/problems/longest-substring-without-repeating-characters/):
  [💡](https://www.youtube.com/watch?v=wiGpQwVHdE0)
  Hashmap (seen, index), `ans = max(ans, i - start + 1)`
- 👀[**Longest Repeating Character Replacement**](https://leetcode.com/problems/longest-repeating-character-replacement/):
  [💡](https://www.youtube.com/watch?v=gqXU1UyA8pk)
  Window with most repeating.
- 👀[**Minimum Window Substring**](https://leetcode.com/problems/minimum-window-substring/):
  [💡](https://www.youtube.com/watch?v=jSto0O4AJbM)
---

- ✅[**Maximum Points You Can Obtain from Cards**](https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/):
  [💡](https://www.youtube.com/watch?v=TsA4vbtfCvo)
  - `cardPoints = [1,2,3,4,5,6,1], k = 3` => `12`
  - Sliding window, minimum subarray of size n-k.
  - O(n) time, O(1) space
- ✅[**Subarray Sum Equals K**](https://leetcode.com/problems/subarray-sum-equals-k/):
  [💡](https://youtu.be/fFVZt-6sgyo)
  - `nums = [1,1,1], k = 2` => `2` ([1, 1] and [1, 1])
  - prefix sum, if increase by k, we found one (subarray in between), initialize `d[0] = 1`
  - O(n) time, O(n) space

## Stack

---

- 👀[**Valid Parentheses**](https://leetcode.com/problems/valid-parentheses/):
  store last opened in stack

---



- ✅[**Reverse Polish Notation**](https://leetcode.com/problems/evaluate-reverse-polish-notation/):
  [💡](https://youtu.be/iu0082c4HDE)
  - `tokens = ["2","1","+","3","*"]` => `9`
  - `b, a = stack.pop(), stack.pop()` ... `return stack[0]`
  - O(2n) time, O(n) space
- ✅[**Decode String**](https://leetcode.com/problems/decode-string/):
  [💡](https://youtu.be/qB0zZpBJlh8)
  - `s = "3[a]2[bc]"` => `"aaabcbc"`
  - append to stack `if != ']'`, pop and multiply
  - O(n_output) time, O(n_output) space

## Binary Search

---

```python
while l <= r:
    mid = (l+r)//2
    if target == nums[mid]
```

- [**Search in Rotated Sorted Array**](https://leetcode.com/problems/search-in-rotated-sorted-array/):
  see which side is sorted.
- [**Find Minimum in Rotated Sorted Array**](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/):
  stop `if nums[l] < nums[r]`.

---

- ✅[**First Bad Version**](https://leetcode.com/problems/first-bad-version/):
  [💡](https://leetcode.com/problems/first-bad-version/solutions/71324/python-understand-easily-from-binary-search-idea/?orderBy=most_votes)
  - `n = 5`, `isBadVersion(3) = false`, `isBadVersion(4) = true`
  - binary search.
  - O(logn) time, O(1) space
- ✅[**Median of Two Sorted Arrays**](https://leetcode.com/problems/median-of-two-sorted-arrays/):
  [💡](https://youtu.be/q6IEA26hvXc)
  - `nums1 = [1,2], nums2 = [3,4]` => `2.5`
  - Binary search on the shorter array until `Aleft <= Bright and Bleft <= Aright`.
  - O(log(m+n)) time, O(1) space
- ✅[**Snapshot Array**](https://leetcode.com/problems/snapshot-array/):
  [💡](https://leetcode.com/problems/snapshot-array/solutions/350562/java-python-binary-search/?orderBy=most_votes)
  - `SnapshotArray(int length)`, `set(index, val)`, `snap()`, `get(index, snap_id)`
  - Dict[int, array], binary search on the list of snapshots.
- ✅[**Random Pick with Weight**](https://leetcode.com/problems/random-pick-with-weight/):
  [💡](https://leetcode.com/problems/random-pick-with-weight/solutions/154044/java-accumulated-freq-sum-binary-search/?orderBy=most_votes)
  - `[1,3]`, `pickIndex()` => `0` with 25% probability, `1` with 75% probability
  - binary search (random (1, total)) on the prefix sum.

## Linked List

---

- ✅[**Merge Two Sorted Lists**](https://leetcode.com/problems/merge-two-sorted-lists/):
  [💡](https://www.youtube.com/watch?v=XIdigk956u0)
  - `l1 = [1,2,4], l2 = [1,3,4]` => `[1,1,2,3,4,4]`
  - dummy first node, curr node, while l1 and l2, add remaining
  - O(n) time, O(1) space
- ✅[**Merge K Sorted Lists**](https://leetcode.com/problems/merge-k-sorted-lists/):
  [💡](https://www.youtube.com/watch?v=q5a5OiGbT6Q)
  - `lists = [[1,4,5],[1,3,4],[2,6]]` => `[1,1,2,3,4,4,5,6]`
  - Merge two lists at a time.
  - O(nlogk) time, O(1) space
- ✅[**Remove Nth Node From End of List**](https://leetcode.com/problems/remove-nth-node-from-end-of-list/):
  [💡](https://www.youtube.com/watch?v=XVuQxVej6y8)
  - `head = [1,2,3,4,5], n = 2` => `[1,2,3,5]`
  - Two pointers, move one n steps ahead.
  - O(n) time, O(1) space
- [**Reverse Linked List**](https://leetcode.com/problems/reverse-linked-list/):
  while cur: ..., tmp, update prev and cur, return prev
- [**Linked List Cycle**](https://leetcode.com/problems/linked-list-cycle/): slow = fast = head, while fast and fast.next:
- [**Reorder List**](https://leetcode.com/problems/reorder-list/)

---

- ✅[**Middle of the Linked List**](https://leetcode.com/problems/middle-of-the-linked-list/):
  [💡]()
  - `1->2->3->4->5` => `3`
  - slow = fast = head, while fast and fast.next: ..., return slow
  - O(n) time, O(1) space
- [**Linked List Cycle II**](https://leetcode.com/problems/linked-list-cycle-ii/): slow1 = head, and intersect with old slow.

## Trees

---

- [**Invert Binary Tree**](https://leetcode.com/problems/invert-binary-tree/):
  `root.right, root.left = self.invertTree(root.left), self.invertTree(root.right) `
- [**Maximum Depth of Binary Tree**](https://leetcode.com/problems/maximum-depth-of-binary-tree/):
  `return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1`
- [**Same Tree**](https://leetcode.com/problems/same-tree/): `isSameTree(p.left, q.left) and isSameTree(p.right, q.right)`
- [**Subtree of Another Tree**](https://leetcode.com/problems/subtree-of-another-tree/): O(n+m)
- [**Binary Tree Level Order Traversal**](https://leetcode.com/problems/binary-tree-level-order-traversal/)
- [**Construct Binary Tree from Preorder and Inorder Traversal**](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
- [**Binary Tree Maximum Path Sum**](https://leetcode.com/problems/binary-tree-maximum-path-sum/)
- [**Serialize and Deserialize Binary Tree**](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

> BST: nodes left < node < nodes right

- [**Lowest Common Ancestor of a Binary Search Tree**](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/):
- [**Validate Binary Search Tree**](https://leetcode.com/problems/validate-binary-search-tree/):
  `validate(node.left, min_val, node.val) and validate(node.right, node.val, max_val)`
- [**Kth Smallest Element in a BST**](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

---

- ✅[**Find Leaves of Binary Tree**](https://www.lintcode.com/problem/650/):
  [💡]
  - dfs, post-order, return layer.

## Tries

---

- [**Implement Trie (Prefix Tree)**](https://leetcode.com/problems/implement-trie-prefix-tree/):
  node with dict[char, node] and a bool isWord.
- [**Design Add And Search Words Data Structure**](https://leetcode.com/problems/design-add-and-search-words-data-structure/):
- [**Word Search II**](https://leetcode.com/problems/word-search-ii/)

---

TODO

## Heap & Priority Queue

---

> Heap invariant: each node is <= than its children.

- [**Find Median from Data Stream**](https://leetcode.com/problems/find-median-from-data-stream/)

---

- ✅[**Stock Price Fluctuation**](https://leetcode.com/problems/stock-price-fluctuation/):
  [💡]
  - `update(int timestamp, int price)`, `current()`, `maximum()`, `minimum()`
  - 2 heaps, timestamps[time, price], self.highest_timestamp.
- [**Kth Largest Element in an Stream**](https://leetcode.com/problems/kth-largest-element-in-a-stream/):
  `while len(self.heap) > k: heapq.heappop(self.heap)`

## Backtracking

---

- [**Combination Sum**](https://leetcode.com/problems/combination-sum/)
- [**Word Search**](https://leetcode.com/problems/word-search/)

---

- [**Sudoku Solver**](https://leetcode.com/problems/sudoku-solver/): `if board[3 * (i // 3) + k // 3][ 3 * (j // 3) + k % 3] == n:`

## Graphs

---

- ✅[**Number of Islands**](https://leetcode.com/problems/number-of-islands/):
  [💡]
  - `grid = [["1", "0"], ["0", "1"]]` => `2`
  - skip visited (mark 0 or set), dfs (4 directions) on 1s.
- [**Clone Graph**](https://leetcode.com/problems/clone-graph/):
  cache of cloned nodes, dfs (neighbors) on original nodes.
- [**Pacific Atlantic Water Flow**](https://leetcode.com/problems/pacific-atlantic-water-flow/):
  start from edge and dfs (4 directions) to higher cells.
- [**Number of Connected Components in an Undirected Graph**](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/):

- [**Graph Valid Tree**](https://www.lintcode.com/problem/graph-valid-tree/description):
  DFS cycle detection, `dfs(0, prev=-1) and n == len(visit)`

---

TODO

## Topological Sort

---

- [**Course Schedule**](https://leetcode.com/problems/course-schedule/):
  BFS topological sort / DFS cycle detection (visited set, graph[c] = []).
- [**Alien Dictionary**](https://www.lintcode.com/problem/892):
  topological sort, DFS cycle detection.

---

- [**Course Schedule II**](https://leetcode.com/problems/course-schedule-ii/):
  ...

## Dynamic Programming

---

https://youtu.be/mBNrRy2_hVs

- Fibonacci: `dp[i] = dp[i-1] + dp[i-2]`
- Zero / One Knapsack:
- Unbounded Knapsack:
- Longest Common Subsequence:
- Palindromes:

### 1D

- ✅[**Coin Change**](https://leetcode.com/problems/coin-change/): 
  [💡](https://www.youtube.com/watch?v=H9bfqozjoqs) Unbounded Knapsack
  - `coins = [1,2,5], amount = 11` => `3`
  - Cache and iterate `range(amount+1)`, recursive is O(coins^amount)
  - O(coins\*amount) time, O(amount) space
- ✅[**Climbing Stairs**](https://leetcode.com/problems/climbing-stairs/):
  [💡](https://www.youtube.com/watch?v=Y0lT9Fck7qI) Fibonacci
  - `n = 2` => `2`, `n = 3` => `3`
  - `temp = n1 + n2`
  - O(n) time, O(1) space
- [**House Robber**](https://leetcode.com/problems/house-robber/):
- [**House Robber II**](https://leetcode.com/problems/house-robber-ii/)
- [**Longest Palindromic Substring**](https://leetcode.com/problems/longest-palindromic-substring/)
- [**Palindrome Substrings**](https://leetcode.com/problems/palindromic-substrings/)
- [**Decode Ways**](https://leetcode.com/problems/decode-ways/)
- [**Maximum Product Subarray**](https://leetcode.com/problems/maximum-product-subarray/)
- [**Word Break**](https://leetcode.com/problems/word-break/)
- [**Longest Increasing Subsequence**](https://leetcode.com/problems/longest-increasing-subsequence/)

---

- ✅[**Student Attendance Record II**](https://leetcode.com/problems/student-attendance-record-ii/):
  Fewer than 2 A, no 3 or more consecutive L.
  [💡]
  - `n = 2` => `8` ("PP", "AP", "PA", "LP", "PL", "AL", "LA", "LL")

### 2D

- ✅[**Unique Paths**](https://leetcode.com/problems/unique-paths/)
  [💡](https://www.youtube.com/watch?v=IlEsdxuD4lY)
  - `m = 3, n = 2` => `3`
  - `cache[i, j] = cache.get((i-1, j), 0) + cache.get((i, j-1), 0)`
  - O(m\*n) time, O(m\*n) space
- [**Longest Common Subsequence**](https://leetcode.com/problems/longest-common-subsequence/)

---

- ✅[**Coin Change 2**](https://leetcode.com/problems/coin-change-2/):
  [💡](https://www.youtube.com/watch?v=Mjy4hd2xgrs)
  - `amount = 5, coins = [1, 2, 5]` => `4`
  - Unbounded Knapsack
- ✅[**Target Sum**](https://leetcode.com/problems/target-sum/):
  [💡](https://www.youtube.com/watch?v=g0npyaQtAQM)
  - assign + or - `nums = [1, 1, 1, 1, 1], target = 3` => `5`
  - 0/1 Knapsack, O(2^n) -> O(n\*sum(nums)))

## Greedy

---

- [**Maximum Subarray**](https://leetcode.com/problems/maximum-subarray/):
  `current_sum = max(current_sum+n, n)`, Kadane.
- [**Jump Game**](https://leetcode.com/problems/jump-game/):
  `if i + nums[i] >= target: target = i`, start from end.

---

TODO

## Intervals

---

- ✅[**Merge Intervals**](https://leetcode.com/problems/merge-intervals/):
  [💡]
  - `intervals = [[1,3],[2,6],[8,10],[15,18]]` => `[[1,6],[8,10],[15,18]]`
- ✅[**Meeting Rooms II**](https://www.lintcode.com/problem/919/):
  [💡]
  min number of conference rooms.
  - `intervals = [(0,30),(5,10),(15,20)]` => `2`
- [**Insert Interval**](https://leetcode.com/problems/insert-interval/):
- [**Non-overlapping Intervals**](https://leetcode.com/problems/non-overlapping-intervals/):
  sort by end, update if start >= end.
- [**Meeting Rooms**](https://www.lintcode.com/problem/920/): if any
  overlap, return false.

---

TODO

## Math & Geometry

---

- [**Rotate Image**](https://leetcode.com/problems/rotate-image/)
- [**Spiral Matrix**](https://leetcode.com/problems/spiral-matrix/)
- [**Set Matrix Zeroes**](https://leetcode.com/problems/set-matrix-zeroes/)

---

- ✅[**Happy Number**](https://leetcode.com/problems/happy-number/):
  [💡]
  - `n = 19` => `true` (`12 + 92 = 82 |...| 12 + 02 + 02 = 1`)
  - Hashset seen or Floyd's cycle detection

## Bit Manipulation

---

[Summary](https://leetcode.com/problems/sum-of-two-integers/discuss/84278/A-summary%3A-how-to-use-bit-manipulation-to-solve-problems-easily-and-efficiently)

- [**Number of 1 Bits**](https://leetcode.com/problems/number-of-1-bits/):
  `res += n % 2; n = n >> 1;` or `n = n & (n - 1); res++;` (rightmost 1)
- [**Counting Bits**](https://leetcode.com/problems/counting-bits/):
  dp with `memo[n] = n % 2 + solve(n / 2, memo);`
- [**Reverse Bits**](https://leetcode.com/problems/reverse-bits/):
- [**Missing Number**](https://leetcode.com/problems/missing-number/):
- [**Sum of Two Integers**](https://leetcode.com/problems/sum-of-two-integers/):

  ---

  TODO

## Extra

---

- [**Network Delay Time**](https://leetcode.com/problems/network-delay-time/):
  Dijkstra.
- [**Path with Maximum Probability**](https://leetcode.com/problems/path-with-maximum-probability/):
  Dijkstra.
- [**Swim in Rising Water**](https://leetcode.com/problems/swim-in-rising-water/):
  Dijkstra.
- [**Shortest Path in Binary Matrix**](https://leetcode.com/problems/shortest-path-in-binary-matrix/):
  A\*.
- [**Find the Index of the First Occurrence in a String**](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/):
  [💡]

  - [KMP](https://www.youtube.com/watch?v=GTJr8OvyEVQ) pattern matching, O(m+n).

- [**Maximum Length of Repeated Subarray**](https://leetcode.com/problems/maximum-length-of-repeated-subarray/):
  Rabin–Karp algorithm / Rolling Hash.
- [**Longest Duplicate Substring**](https://leetcode.com/problems/longest-duplicate-substring/) / [s](https://leetcode.com/problems/longest-duplicate-substring/solutions/695029/python-binary-search-o-n-log-n-average-with-rabin-karp-explained/):
  Rabin Karp + Binary Search.
- [**Edit Distance**](https://leetcode.com/problems/edit-distance/) / [s](https://leetcode.com/problems/edit-distance/solutions/1475220/python-3-solutions-top-down-dp-bottom-up-dp-o-n-in-space-clean-concise/):
  DP.
