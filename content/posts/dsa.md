---
title: "📝 Data structures and algorithms"
date: 2022-10-27
draft: true
TocOpen: true

tags: ["Software Engineering", "Programming", "Python"]
---

Some notes about DSA

<!--more-->

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

### Arrays & Hashing

- ✅[**Contains Duplicate**](https://leetcode.com/problems/contains-duplicate/):
  Naive, Sort, Set
- ✅[**Valid Anagram**](https://leetcode.com/problems/valid-anagram/):
  Two (one) Hashmap, Sort
- ✅[**Two Sum**](https://leetcode.com/problems/two-sum/):
  Hashmap (seen)
- ✅[**Group Anagrams**](https://leetcode.com/problems/group-anagrams/):
  Hashmap (counter as key)
- ✅[**Top K Frequent Elements**](https://leetcode.com/problems/top-k-frequent-elements/):
  Count, List of frequency `groups_by_freq = [[] for _ in range(len(nums)+1)]`
- ✅[**Product of Array Except Self**](https://leetcode.com/problems/product-of-array-except-self/):
  Multiply all and divide by each / Prefix product, Suffix product.
- ✅[**Encode and Decode Strings**](https://www.lintcode.com/problem/659/):
  Length + Separator
- ✅[**Longest Consecutive Sequence**](https://leetcode.com/problems/longest-consecutive-sequence/):
  Set and start if n-1 not in set

**Other**:

- ✅[**Valid Sudoku**](https://leetcode.com/problems/valid-sudoku/): `squares[(r // 3, c // 3)].add(board[r][c])`

### Two Pointers

- ✅[**Valid Palindrome**](https://leetcode.com/problems/valid-palindrome/):
  2 pointers
- ✅[**3Sum**](https://leetcode.com/problems/3sum/) / [s](https://github.com/neetcode-gh/leetcode/blob/main/python/15-3Sum.py):
  sort and use 2 pointers, from n^3 to n^2, or go case by case (1 zero on the list, 3 zeros, 2 neg 1 pos, 2 pos 1 neg).

  ```python
  res = set()
  nums.sort()

  for i, a in enumerate(nums):
      l, r = i + 1, len(nums) - 1
      while l < r:
          threeSum = a + nums[l] + nums[r]
          if threeSum > 0: r -= 1
          elif threeSum < 0: l += 1
          else:
              res.add((a, nums[l], nums[r]))
              l += 1
  return res
  ```

- ✅[**Container With Most Water**](https://leetcode.com/problems/container-with-most-water/):
  start from both ends, move the smaller one

**Other**:

- ✅[**Two Sum II**](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
- [**Trapping Rain Water**](https://leetcode.com/problems/trapping-rain-water/):
  DP, can optimize to O(1) space by using 2 pointers
- ✅[**String Compression**](https://leetcode.com/problems/string-compression/):
  2 pointers, slow and fast.

### Sliding Window

- [**Best Time to Buy and Sell Stock**](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/):
  [Kadane's algorithm](https://raw.githubusercontent.com/neetcode-gh/leetcode/main/python/121-Best-Time-To-Buy-and-Sell-Stock.py)
  ```python
  l = 0
  for r in range(1, len(prices)):
      if prices[r] < prices[l]:
          l = r
      res = max(res, prices[r] - prices[l])
  return res
  ```
- [**Longest Substring Without Repeating Characters**](https://leetcode.com/problems/longest-substring-without-repeating-characters/):
  Hashmap (seen, index), `ans = max(ans, i - start + 1)`
- [**Longest Repeating Character Replacement**](https://leetcode.com/problems/longest-repeating-character-replacement/):
  Find the window with most repeating characters and `end - start + 1 - maxCount < k`, `ans = max(ans, end - start + 1)`
- [**Minimum Window Substring**](https://leetcode.com/problems/minimum-window-substring/):
  ???

**Other**:

TODO

### Stack

- ✅[**Valid Parentheses**](https://leetcode.com/problems/valid-parentheses/):
  store last opened in stack

**Other**:

TODO

### Binary Search

```python
while l <= r:
    mid = (l+r)//2
    if target == nums[mid]
```

- [**Search in Rotated Sorted Array**](https://leetcode.com/problems/search-in-rotated-sorted-array/):
  see which side is sorted.
- [**Find Minimum in Rotated Sorted Array**](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/):
  stop `if nums[l] < nums[r]`.

**Other**:

TODO

### Linked List

- [**Reverse Linked List**](https://leetcode.com/problems/reverse-linked-list/):
  while cur: ..., return prev
- [**Merge Two Sorted Lists**](https://leetcode.com/problems/merge-two-sorted-lists/)
- [**Reorder List**](https://leetcode.com/problems/reorder-list/)
- [**Remove Nth Node From End of List**](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
- [**Linked List Cycle**](https://leetcode.com/problems/linked-list-cycle/)
- [**Merge K Sorted Lists**](https://leetcode.com/problems/merge-k-sorted-lists/)

**Other**:

TODO

### Trees

- [**Invert Binary Tree**](https://leetcode.com/problems/invert-binary-tree/):
  `root.right, root.left = self.invertTree(root.left), self.invertTree(root.right) `
- [**Maximum Depth of Binary Tree**](https://leetcode.com/problems/maximum-depth-of-binary-tree/):
  `return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1`
- [**Same Tree**](https://leetcode.com/problems/same-tree/): `isSameTree(p.left, q.left) and isSameTree(p.right, q.right)`
- [**Subtree of Another Tree**](https://leetcode.com/problems/subtree-of-another-tree/): O(n+m)

  ```python
  if self.sameTree(s, t):
    return True
  return self.isSubtree(s.left, t) or self.isSubtree(s.right, t)
  ```

- [**Lowest Common Ancestor of a Binary Search Tree**](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)
- [**Binary Tree Level Order Traversal**](https://leetcode.com/problems/binary-tree-level-order-traversal/)
- [**Validate Binary Search Tree**](https://leetcode.com/problems/validate-binary-search-tree/)
- [**Kth Smallest Element in a BST**](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)
- [**Construct Binary Tree from Preorder and Inorder Traversal**](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
- [**Binary Tree Maximum Path Sum**](https://leetcode.com/problems/binary-tree-maximum-path-sum/)
- [**Serialize and Deserialize Binary Tree**](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

**Other**:

TODO

### Tries

- [**Implement Trie (Prefix Tree)**](https://leetcode.com/problems/implement-trie-prefix-tree/):
  node with dict[char, node] and a bool isWord.
- [**Design Add And Search Words Data Structure**](https://leetcode.com/problems/design-add-and-search-words-data-structure/):
- [**Word Search II**](https://leetcode.com/problems/word-search-ii/)

**Other**:

TODO

### Heap & Priority Queue

- [**Find Median from Data Stream**](https://leetcode.com/problems/find-median-from-data-stream/)

**Other**:

TODO

### Backtracking

- [**Combination Sum**](https://leetcode.com/problems/combination-sum/)
- [**Word Search**](https://leetcode.com/problems/word-search/)

**Other**:

- [**Sudoku Solver**](https://leetcode.com/problems/sudoku-solver/): `if board[3 * (i // 3) + k // 3][ 3 * (j // 3) + k % 3] == n:`

  ```python
  def solveSudoku(self, board: List[List[str]]) -> None:
    for i in range(9):
        for j in range(9):
            if board[i][j] != ".":
                continue
            for n in range(1, 10):
                n = str(n)
                if self.is_valid(board, i, j, n):
                    board[i][j] = n
                    if self.solveSudoku(board):
                        return True
                    board[i][j] = "."
            return False
    return True

  def is_valid(self, board, i, j, n):
    for k in range(9):
        if board[i][k] == n: return False
        if board[k][j] == n: return False
        if board[3 * (i // 3) + k // 3][ 3 * (j // 3) + k % 3] == n:
            return False
    return True
  ```

### Graphs

- [**Number of Islands**](https://leetcode.com/problems/number-of-islands/):
  skip visited (mark 0 or set), dfs (4 directions) on 1s.
- [**Clone Graph**](https://leetcode.com/problems/clone-graph/):
  cache of cloned nodes, dfs (neighbors) on original nodes.
- [**Pacific Atlantic Water Flow**](https://leetcode.com/problems/pacific-atlantic-water-flow/):
  start from edge and dfs (4 directions) to higher cells.
- [**Course Schedule**](https://leetcode.com/problems/course-schedule/):
  BFS topological sort / DFS cycle detection (visited set, graph[c] = []).
- [**Number of Connected Components in an Undirected Graph**](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/):

  ```python
  class UnionFind:
      def __init__(self):
          self.f = {}
      def findParent(self, x):
          y = self.f.get(x, x)
          if x != y:
              y = self.f[x] = self.findParent(y)
          return y
      def union(self, x, y):
          self.f[self.findParent(x)] = self.findParent(y)

  class Solution:
      def countComponents(self, n: int, edges: List[List[int]]) -> int:
          dsu = UnionFind()
          for a, b in edges:
              dsu.union(a, b)
          return len(set(dsu.findParent(x) for x in range(n)))
  ```

- [**Graph Valid Tree**](https://leetcode.com/problems/graph-valid-tree/):
  DFS cycle detection, `dfs(0, prev=-1) and n == len(visit)`

**Other**:

TODO

### Advanced Graphs

- [**Alien Dictionary**](https://www.lintcode.com/problem/892):
  topological sort, DFS cycle detection.

**Other**:

TODO

### 1-D Dynamic Programming

- [**Climbing Stairs**](https://leetcode.com/problems/climbing-stairs/):
  fibonnaci, `temp = n1 + n2`
- [**House Robber**](https://leetcode.com/problems/house-robber/):
  ```python
  for n in nums:
    temp = max(n + rob1, rob2)
    rob1 = rob2
    rob2 = temp
  ```
- [**House Robber II**](https://leetcode.com/problems/house-robber-ii/)
- [**Longest Palindromic Substring**](https://leetcode.com/problems/longest-palindromic-substring/)
- [**Palindrome Substrings**](https://leetcode.com/problems/palindromic-substrings/)
- [**Decode Ways**](https://leetcode.com/problems/decode-ways/)
- [**Coin Change**](https://leetcode.com/problems/coin-change/)
- [**Maximum Product Subarray**](https://leetcode.com/problems/maximum-product-subarray/)
- [**Word Break**](https://leetcode.com/problems/word-break/)
- [**Longest Increasing Subsequence**](https://leetcode.com/problems/longest-increasing-subsequence/)

**Other**:

TODO

### 2-D Dynamic Programming

- [**Unique Paths**](https://leetcode.com/problems/unique-paths/)
- [**Longest Common Subsequence**](https://leetcode.com/problems/longest-common-subsequence/)

**Other**:

TODO

### Greedy

- [**Maximum Subarray**](https://leetcode.com/problems/maximum-subarray/):
  `current_sum = max(current_sum+n, n)`
- [**Jump Game**](https://leetcode.com/problems/jump-game/):
  `if i + nums[i] >= target: target = i`

**Other**:

TODO

### Intervals

- [**Insert Interval**](https://leetcode.com/problems/insert-interval/):
  ```python
  if interval.end < newInterval.start:
  elif interval.start > newInterval.end:
  else:
    newInterval = [min, max]
  res.append(newInterval)
  ```
- [**Merge Intervals**](https://leetcode.com/problems/merge-intervals/):
  ```python
  for start, end in sorted(intervals):
    if not result or result[-1][1] < start:
        result.append([start, end])
    elif end > result[-1][1]:
        result[-1][1] = end
  ```
- [**Non-overlapping Intervals**](https://leetcode.com/problems/non-overlapping-intervals/):
  sort by end, update if start >= end.
- [**Meeting Rooms**](https://www.lintcode.com/problem/920/): if any
  overlap, return false.
- [**Meeting Rooms II**](https://www.lintcode.com/problem/919/):

  ```python
  start = sorted([i.start for i in intervals])
  end = sorted([i.end for i in intervals])

  res, count = 0, 0
  s, e = 0, 0
  while s < len(intervals):
      if start[s] < end[e]:
          s += 1
          count += 1
      else:
          e += 1
          count -= 1
      res = max(res, count)
  return res
  ```

**Other**:

TODO

### Math & Geometry

- [**Rotate Image**](https://leetcode.com/problems/rotate-image/)

  ```python
  l, r = 0, len(matrix) - 1
  while l < r:
      for i in range(r - l):
          top, bottom = l, r

          # save the topleft
          topLeft = matrix[top][l + i]

          # move bottom left into top left
          matrix[top][l + i] = matrix[bottom - i][l]

          # move bottom right into bottom left
          matrix[bottom - i][l] = matrix[bottom][r - i]

          # move top right into bottom right
          matrix[bottom][r - i] = matrix[top + i][r]

          # move top left into top right
          matrix[top + i][r] = topLeft
      r -= 1
      l += 1
  ```

- [**Spiral Matrix**](https://leetcode.com/problems/spiral-matrix/)

  ```python
  res = []
  left, right = 0, len(matrix[0])
  top, bottom = 0, len(matrix)

  while left < right and top < bottom:
      # get every i in the top row
      for i in range(left, right):
          res.append(matrix[top][i])
      top += 1
      # get every i in the right col
      for i in range(top, bottom):
          res.append(matrix[i][right - 1])
      right -= 1
      if not (left < right and top < bottom):
          break
      # get every i in the bottom row
      for i in range(right - 1, left - 1, -1):
          res.append(matrix[bottom - 1][i])
      bottom -= 1
      # get every i in the left col
      for i in range(bottom - 1, top - 1, -1):
          res.append(matrix[i][left])
      left += 1

  return res
  ```

- [**Set Matrix Zeroes**](https://leetcode.com/problems/set-matrix-zeroes/)

**Other**:

TODO

### [Bit Manipulation](https://leetcode.com/problems/sum-of-two-integers/discuss/84278/A-summary%3A-how-to-use-bit-manipulation-to-solve-problems-easily-and-efficiently)

- [**Number of 1 Bits**](https://leetcode.com/problems/number-of-1-bits/):
  `res += n % 2; n = n >> 1;` or `n = n & (n - 1); res++;` (rightmost 1)
- [**Counting Bits**](https://leetcode.com/problems/counting-bits/):
  dp with `memo[n] = n % 2 + solve(n / 2, memo);`
- [**Reverse Bits**](https://leetcode.com/problems/reverse-bits/):

  ```python
  for i in range(32):
    bit = (n >> i) & 1  # is it a 1?
    res += (bit << (31 - i)) # shift it to the left
  ```

- [**Missing Number**](https://leetcode.com/problems/missing-number/):

  ```python
  for i, n in enumerate(nums):
      res ^= n
      res ^= i+1
  return res
  ```

- [**Sum of Two Integers**](https://leetcode.com/problems/sum-of-two-integers/):

  ```python
  while b:
      a, b = a ^ b, (a & b) << 1
  return a
  ```

  **Other**:

  TODO

### Extra

- [**Path with Minimum Effort**](https://leetcode.com/problems/path-with-minimum-effort/):
  Dijkstra.

  ```python
  dist, heap = {...}, [(0, 0, 0)]

  while heap:
      d, i, j = heapq.heappop(heap)
      if i == len(heights)-1 and j == len(heights[0])-1: return d
      for ...:
          newDist = ...
          if newDist < dist[ii,jj]:
              dist[ii,jj] = newDist
              heapq.heappush(heap, (newDist, ii, jj))
  ```

- [**Cheapest Flights Within K Stops**](https://leetcode.com/problems/cheapest-flights-within-k-stops/):
  Dijkstra.
- [**Shortest Path in Binary Matrix**](https://leetcode.com/problems/shortest-path-in-binary-matrix/):
  A\*.
- [**Find the Index of the First Occurrence in a String**](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/):
  [KMP](https://www.youtube.com/watch?v=GTJr8OvyEVQ) pattern matching, O(m+n).

  ```python
  def kmp_table(self, needle):
      table = [-1]+[0]*len(needle)
      i, j = 1, 0
      while i < len(needle):
          if j == -1 or needle[i] == needle[j]:   
              i += 1
              j += 1
              table[i] = j
          else:
              j = table[j]
      return table

  def strStr(self, haystack: str, needle: str) -> int:
      table = self.kmp_table(needle)
      i, j = 0, 0
      while i < len(haystack) and j < len(needle):
          if j == -1 or haystack[i] == needle[j]:
              i += 1
              j += 1
          else:
              j = table[j]
      return i-j if j == len(needle) else -1
  ```

- [**Maximum Length of Repeated Subarray**](https://leetcode.com/problems/maximum-length-of-repeated-subarray/):
  Rabin–Karp algorithm / Rolling Hash.
- [**Longest Duplicate Substring**](https://leetcode.com/problems/longest-duplicate-substring/) / [s](https://leetcode.com/problems/longest-duplicate-substring/solutions/695029/python-binary-search-o-n-log-n-average-with-rabin-karp-explained/):
  Rabin Karp + Binary Search.
- [**Edit Distance**](https://leetcode.com/problems/edit-distance/) / [s](https://leetcode.com/problems/edit-distance/solutions/1475220/python-3-solutions-top-down-dp-bottom-up-dp-o-n-in-space-clean-concise/):
  DP.
