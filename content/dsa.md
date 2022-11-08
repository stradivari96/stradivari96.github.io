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

- ✅[**Two Sum**](https://leetcode.com/problems/two-sum/):
  - `nums = [2,7,11,15], target = 9` => `[0,1]`
  - HashMap `{n: i}`
- [**Contains Duplicate**](https://leetcode.com/problems/contains-duplicate/):
  Naive, Sort, Set
- [**Valid Anagram**](https://leetcode.com/problems/valid-anagram/):
  Two (one) Hashmap, Sort
- [**Group Anagrams**](https://leetcode.com/problems/group-anagrams/):
  Hashmap (counter as key)
- [**Top K Frequent Elements**](https://leetcode.com/problems/top-k-frequent-elements/):
  Count, List of frequency `groups_by_freq = [[] for _ in range(len(nums)+1)]`
- [**Product of Array Except Self**](https://leetcode.com/problems/product-of-array-except-self/):
  Prefix product, Suffix product.
- [**Encode and Decode Strings**](https://www.lintcode.com/problem/659/):
  Length + Separator
- [**Longest Consecutive Sequence**](https://leetcode.com/problems/longest-consecutive-sequence/):
  Set and start if n-1 not in set

**Other**:

- ✅[**Minimum Time Difference**](https://leetcode.com/problems/minimum-time-difference/):
  - `timePoints = ["23:59","00:00"]` => `1`
  - map to minutes & sort, `(time[i]-time[i-1])%(24*60)`
- ✅[**Logger Rate Limiter**](https://gist.github.com/kuntalchandra/7822e388e0d2d78ec27f566266584b49):
  - `shouldPrintMessage(1, "foo"), shouldPrintMessage(3, "foo")` => `true, false`
  - Hashmap `{message: timestamp}`, `if self.cache[message] + 10 > timestamp`
- ✅[**Number of matching subsequence**](https://leetcode.com/problems/number-of-matching-subsequences/):
  - `s = "abcde", words = ["a","bb","acd","ace"]` => `3`
  - Hashmap `{char: [words]}`.
- ✅[**Text Justification**](https://leetcode.com/problems/text-justification/):
  ```python
  for w in words:
      if num_of_letters + len(w) + len(cur) > maxWidth:
          for i in range(maxWidth - num_of_letters):
              cur[i%(len(cur)-1 or 1)] += ' '
          res.append(''.join(cur))
          cur, num_of_letters = [], 0
      cur += [w]
      num_of_letters += len(w)
  return res + [' '.join(cur).ljust(maxWidth)]
  ```
- [**Valid Sudoku**](https://leetcode.com/problems/valid-sudoku/): `squares[(r // 3, c // 3)].add(board[r][c])`
- [**Close strings**](https://leetcode.com/problems/determine-if-two-strings-are-close): same set of chars and same count.values()
- [**Find original array from doubled array**](https://leetcode.com/problems/find-original-array-from-doubled-array/):
  Hashmap, check if 2x in map, if not, check if x/2 in map.
  ```python
  cnt, ans = Counter(changed), []
  if len(changed) % 2: return []
  for x in sorted(cnt.keys()):
      if cnt[x] > cnt[x * 2]: return []
      if x == 0:
          if cnt[x] % 2: return []
          else: ans += [0] * (cnt[x] // 2)
      else:
          ans += [x] * cnt[x]
      cnt[2 * x] -= cnt[x]
  return ans
  ```

## Two Pointers

---

- [**Valid Palindrome**](https://leetcode.com/problems/valid-palindrome/):
  2 pointers
- [**3Sum**](https://leetcode.com/problems/3sum/) / [s](https://github.com/neetcode-gh/leetcode/blob/main/python/15-3Sum.py):
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

- [**Container With Most Water**](https://leetcode.com/problems/container-with-most-water/):
  start from both ends, move the smaller one

**Other**:

- [**Two Sum II**](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/):
  2 pointers.
- [**Trapping Rain Water**](https://leetcode.com/problems/trapping-rain-water/):
  DP, can optimize to O(1) space by using 2 pointers
- [**String Compression**](https://leetcode.com/problems/string-compression/):
  2 pointers, slow and fast.

## Sliding Window

---

- [**Best Time to Buy and Sell Stock**](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/):
  [Kadane's algorithm](https://raw.githubusercontent.com/neetcode-gh/leetcode/main/python/121-Best-Time-To-Buy-and-Sell-Stock.py)
  ```python
  for p in prices:
      min_price = min(min_price, p)
      res = max(res, p-min_price)
  return res
  ```
- [**Longest Substring Without Repeating Characters**](https://leetcode.com/problems/longest-substring-without-repeating-characters/):
  Hashmap (seen, index), `ans = max(ans, i - start + 1)`
  ```python
  for i, c in enumerate(s):
      if c in seen and start <= seen[c]: start = seen[c] + 1
      else: ans = max(ans, i - start + 1)
      seen[c] = i
  ```
- [**Longest Repeating Character Replacement**](https://leetcode.com/problems/longest-repeating-character-replacement/):
  Window with most repeating.
  ```python
  res, l, maxf = 0, 0, 0
  for r in range(len(s)):
      count[s[r]] = 1 + count.get(s[r], 0)
      maxf = max(maxf, count[s[r]])
      if (r - l + 1) - maxf > k:
          count[s[l]] -= 1; l += 1
      res = max(res, r - l + 1)
  ```
- [**Minimum Window Substring**](https://leetcode.com/problems/minimum-window-substring/):
  ```python
  min_len = float("inf")
  min_left = 0
  win_start = 0
  win_count = {}
  for i, c in enumerate(s):
      win_count[c] = win_count.get(c, 0)+1
      while all(win_count.get(char, 0) >= n for char, n in count_t.items()):
          # update result and shrink window
  ```

**Other**:

- ✅[**Maximum Points You Can Obtain from Cards**](https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/):
  - `cardPoints = [1,2,3,4,5,6,1], k = 3` => `12`
  - Sliding window, minimum subarray of size n-k.
- ✅[**Subarray Sum Equals K**](https://leetcode.com/problems/subarray-sum-equals-k/):

  - nums = [1,1,1], k = 2

  ```python
  count = 0; sums = 0
  d = {0: 1}

  # prefix sum increase by k
  for i in range(len(nums)):
      sums += nums[i]
      count += d.get(sums-k,0)
      d[sums] = d.get(sums,0) + 1

  return count
  ```

## Stack

---

- [**Valid Parentheses**](https://leetcode.com/problems/valid-parentheses/):
  store last opened in stack

**Other**:

- ✅[**Reverse Polish Notation**](https://leetcode.com/problems/evaluate-reverse-polish-notation/):
  - `tokens = ["2","1","+","3","*"]` => `9`
  - `b, a = stack.pop(), stack.pop()` ... `return stack[0]`
- ✅[**Decode String**](https://leetcode.com/problems/decode-string/):
  - `s = "3[a]2[bc]"` => `"aaabcbc"`
  ```python
  stack = []; curNum = 0; curString = ''
  for c in s:
      if c == '[':
          stack.append(curString)
          stack.append(curNum)
          curString = ''
          curNum = 0
      elif c == ']':
          num = stack.pop()
          prevString = stack.pop()
          curString = prevString + num*curString
      elif c.isdigit():
          curNum = curNum*10 + int(c)
      else:
          curString += c
  ```

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

**Other**:

- ✅[**Snapshot Array**](https://leetcode.com/problems/snapshot-array/):
  Dict[int, array], binary search on the list of snapshots.
- ✅[**Random Pick with Weight**](https://leetcode.com/problems/random-pick-with-weight/):
  binary search (random (1, total)) on the prefix sum.
- ✅[**First Bad Version**](https://leetcode.com/problems/first-bad-version/):
  binary search.

## Linked List

---

- [**Reverse Linked List**](https://leetcode.com/problems/reverse-linked-list/):
  while cur: ..., tmp, update prev and cur, return prev
- [**Linked List Cycle**](https://leetcode.com/problems/linked-list-cycle/): slow = fast = head, while fast and fast.next:
- [**Merge Two Sorted Lists**](https://leetcode.com/problems/merge-two-sorted-lists/)
- [**Reorder List**](https://leetcode.com/problems/reorder-list/)
- [**Remove Nth Node From End of List**](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
- [**Merge K Sorted Lists**](https://leetcode.com/problems/merge-k-sorted-lists/)

**Other**:

- [**Linked List Cycle II**](https://leetcode.com/problems/linked-list-cycle-ii/): slow1 = head, and intersect with old slow.

## Trees

---

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

- [**Binary Tree Level Order Traversal**](https://leetcode.com/problems/binary-tree-level-order-traversal/)
- [**Construct Binary Tree from Preorder and Inorder Traversal**](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
- [**Binary Tree Maximum Path Sum**](https://leetcode.com/problems/binary-tree-maximum-path-sum/)
- [**Serialize and Deserialize Binary Tree**](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

> BST: nodes left < node < nodes right

- [**Lowest Common Ancestor of a Binary Search Tree**](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/):
  ```python
  while cur:
      if p.val < cur.val and q.val < cur.val: cur = cur.left
      elif p.val > cur.val and q.val > cur.val: cur = cur.right
      else: return cur
  ```
- [**Validate Binary Search Tree**](https://leetcode.com/problems/validate-binary-search-tree/):
  `validate(node.left, min_val, node.val) and validate(node.right, node.val, max_val)`
- [**Kth Smallest Element in a BST**](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

**Other**:

- ✅[**Find Leaves of Binary Tree**](https://www.lintcode.com/problem/650/): dfs, post-order, return layer.
  ```python
  def dfs(node, layer):
      if not node: return layer
      left = dfs(node.left, layer)
      right = dfs(node.right, layer)
      layer = max(left, right)
      if len(result) <= layer: result.append([])
      result[layer].append(node.val)
      return layer + 1
  dfs(root, 0)
  ```

## Tries

---

- [**Implement Trie (Prefix Tree)**](https://leetcode.com/problems/implement-trie-prefix-tree/):
  node with dict[char, node] and a bool isWord.
- [**Design Add And Search Words Data Structure**](https://leetcode.com/problems/design-add-and-search-words-data-structure/):
- [**Word Search II**](https://leetcode.com/problems/word-search-ii/)

**Other**:

TODO

## Heap & Priority Queue

---

> Heap invariant: each node is <= than its children.

- [**Find Median from Data Stream**](https://leetcode.com/problems/find-median-from-data-stream/)

  ```python
  def __init__(self):
        self.small, self.large = [], []  # maxHeap, minHeap (python default)

    def addNum(self, num: int) -> None:
        heapq.heappush(self.small, -1 * num)

        if self.small and self.large and (-1 * self.small[0]) > self.large[0]:
            val = -1 * heapq.heappop(self.small)
            heapq.heappush(self.large, val)

        if len(self.small) > len(self.large) + 1:
            val = -1 * heapq.heappop(self.small)
            heapq.heappush(self.large, val)
        if len(self.large) > len(self.small) + 1:
            val = heapq.heappop(self.large)
            heapq.heappush(self.small, -1 * val)

    def findMedian(self) -> float:
        if len(self.small) > len(self.large):
            return -1 * self.small[0]
        elif len(self.large) > len(self.small):
            return self.large[0]
        return (-1 * self.small[0] + self.large[0]) / 2
  ```

**Other**:

- ✅[**Stock Price Fluctuation**](https://leetcode.com/problems/stock-price-fluctuation/):
  - `update(int timestamp, int price)`, `current()`, `maximum()`, `minimum()`
  - 2 heaps, timestamps[time, price], self.highest_timestamp.
- [**Kth Largest Element in an Stream**](https://leetcode.com/problems/kth-largest-element-in-a-stream/):
  `while len(self.heap) > k: heapq.heappop(self.heap)`

## Backtracking

---

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

## Graphs

---

- ✅[**Number of Islands**](https://leetcode.com/problems/number-of-islands/):
  - `grid = [["1", "0"], ["0", "1"]]` => `2`
  - skip visited (mark 0 or set), dfs (4 directions) on 1s.
- [**Clone Graph**](https://leetcode.com/problems/clone-graph/):
  cache of cloned nodes, dfs (neighbors) on original nodes.
- [**Pacific Atlantic Water Flow**](https://leetcode.com/problems/pacific-atlantic-water-flow/):
  start from edge and dfs (4 directions) to higher cells.
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

- [**Graph Valid Tree**](https://www.lintcode.com/problem/graph-valid-tree/description):
  DFS cycle detection, `dfs(0, prev=-1) and n == len(visit)`

**Other**:

TODO

## Topological Sort

---

- [**Course Schedule**](https://leetcode.com/problems/course-schedule/):
  BFS topological sort / DFS cycle detection (visited set, graph[c] = []).
- [**Alien Dictionary**](https://www.lintcode.com/problem/892):
  topological sort, DFS cycle detection.

**Other**:

- [**Course Schedule II**](https://leetcode.com/problems/course-schedule-ii/):
  ...

## 1-D Dynamic Programming

---

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

- ✅[**Student Attendance Record II**](https://leetcode.com/problems/student-attendance-record-ii/):
  - `n = 2` => `8` ("PP", "AP", "PA", "LP", "PL", "AL", "LA", "LL")
  ```python
  if n == 1: return 3
  if n == 0: return 0
  nums = [1, 1, 2]
  i = 2
  while i < n:
      nums.append((nums[i] + nums[i-1] + nums[i-2])% 1000000007)
      i += 1
  result = (nums[n] + nums[n-1] + nums[n-2]) % 1000000007
  for i in range(n):
      result += nums[i+1] * nums[n-i] % 1000000007
      result %= 1000000007
  return result
  ```

## 2-D Dynamic Programming

---

- ✅[**Unique Paths**](https://leetcode.com/problems/unique-paths/)
  - `m = 3, n = 2` => `3`
  ```python
  def uniquePaths(self, m, n):
      dp = [[1]*n for i in range(m)]
      for i, j in product(range(1, m), range(1, n)):
          dp[i][j] = dp[i-1][j] + dp[i][j-1]
      return dp[-1][-1]
  ```
- [**Longest Common Subsequence**](https://leetcode.com/problems/longest-common-subsequence/)

**Other**:

TODO

## Greedy

---

- [**Maximum Subarray**](https://leetcode.com/problems/maximum-subarray/):
  `current_sum = max(current_sum+n, n)`, Kadane.
- [**Jump Game**](https://leetcode.com/problems/jump-game/):
  `if i + nums[i] >= target: target = i`, start from end.

**Other**:

TODO

## Intervals

---

- ✅[**Merge Intervals**](https://leetcode.com/problems/merge-intervals/):
  - `intervals = [[1,3],[2,6],[8,10],[15,18]]` => `[[1,6],[8,10],[15,18]]`
  ```python
  for start, end in sorted(intervals):
    if not result or result[-1][1] < start:
        result.append([start, end])
    elif end > result[-1][1]:
        result[-1][1] = end
  ```
- ✅[**Meeting Rooms II**](https://www.lintcode.com/problem/919/): min number of conference rooms.

  - `intervals = [(0,30),(5,10),(15,20)]` => `2`

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

- [**Insert Interval**](https://leetcode.com/problems/insert-interval/):

  ```python
  if interval.end < newInterval.start:
  elif interval.start > newInterval.end:
  else:
    newInterval = [min, max]
  res.append(newInterval)
  ```

- [**Non-overlapping Intervals**](https://leetcode.com/problems/non-overlapping-intervals/):
  sort by end, update if start >= end.
- [**Meeting Rooms**](https://www.lintcode.com/problem/920/): if any
  overlap, return false.

**Other**:

TODO

## Math & Geometry

---

- [**Rotate Image**](https://leetcode.com/problems/rotate-image/)

  ```python
  l, r = 0, len(matrix) - 1
  while l < r:
      for i in range(r - l):
          top, bottom = l, r
          topLeft = matrix[top][l + i]
          matrix[top][l + i] = matrix[bottom - i][l]
          matrix[bottom - i][l] = matrix[bottom][r - i]
          matrix[bottom][r - i] = matrix[top + i][r]
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
      for i in range(left, right):
          res.append(matrix[top][i])
      top += 1
      for i in range(top, bottom):
          res.append(matrix[i][right - 1])
      right -= 1
      if not (left < right and top < bottom):
          break
      for i in range(right - 1, left - 1, -1):
          res.append(matrix[bottom - 1][i])
      bottom -= 1
      for i in range(bottom - 1, top - 1, -1):
          res.append(matrix[i][left])
      left += 1

  return res
  ```

- [**Set Matrix Zeroes**](https://leetcode.com/problems/set-matrix-zeroes/)

**Other**:

- ✅[**Happy Number**](https://leetcode.com/problems/happy-number/):
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
