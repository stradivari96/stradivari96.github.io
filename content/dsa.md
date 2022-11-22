---
title: "📝 Data structures and algorithms"
date: 2022-10-27
searchHidden: true
robotsNoIndex: true
---

Some notes about DSA

<!--more-->

<script>
    function openRandomLink() {
        var links = document.querySelectorAll("a");
        links = Array.from(links)
        links = links.filter(a => a.href.startsWith("https://leetcode.com/problems/") && !a.href.includes("solutions"));
        var randomLink = links[Math.floor(Math.random() * links.length)];
        randomLink.scrollIntoView();
        window.open(randomLink.href, "_blank");
    }
    window.onload = function() {
        var links = document.querySelectorAll("a");
        links = Array.from(links)
        links = links.filter(a => a.href.startsWith("https://leetcode.com/") || a.href.includes("youtu"));
        links.forEach(a => a.target = "_blank");
    }
</script>

<button onclick="openRandomLink()" style="border: solid; border-width:1px" >
Random problem
</button>


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
  - Hashmap seen with index
  - O(n) time, O(n) space
- 👀[**Contains Duplicate**](https://leetcode.com/problems/contains-duplicate/):
  [💡](https://www.youtube.com/watch?v=3OamzN90kPg)
  - `nums = [1,2,3,1]` => `true`
  - Hashmap seen
  - O(n) time, O(n) space
- 👀[**Valid Anagram**](https://leetcode.com/problems/valid-anagram/):
  [💡](https://www.youtube.com/watch?v=9UtInBqnCgA)
  - `s = "anagram", t = "nagaram"` => `true`
  - Counter for each string
  - O(s+t) time, O(1) space
- 👀[**Group Anagrams**](https://leetcode.com/problems/group-anagrams/):
  [💡](https://www.youtube.com/watch?v=vzdNOK2oB2E)
  - `strs = ["tea","tan","ate","nat"]` => `[["nat","tan"],["ate","tea"]]`
  - Counter for each string, use tuple as key
  - O(strings*average_lenth) time, O(strings) space
- 👀[**Top K Frequent Elements**](https://leetcode.com/problems/top-k-frequent-elements/):
  [💡](https://www.youtube.com/watch?v=YPTqKIgVk-k)
  - `nums = [1,1,1,2,2,3], k = 2` => `[1,2]`
  - Counter, list of frequencies (len(nums)), iterate from end
  - O(n) time, O(n) space
- 👀[**Product of Array Except Self**](https://leetcode.com/problems/product-of-array-except-self/):
  [💡](https://www.youtube.com/watch?v=bNvIQI2wAjk)
  - `nums = [1,2,3,4]` => `[24,12,8,6]`
  - Prefix product, Suffix product.
  - O(n) time, O(1) space
- 👀[**Encode and Decode Strings**](https://www.lintcode.com/problem/659/):
  [💡](https://www.youtube.com/watch?v=B1k_sxOSgv8)
  - Create single string and split it back
  - Length + Separator
  - O(n) encode, O(n) decode
- 👀[**Longest Consecutive Sequence**](https://leetcode.com/problems/longest-consecutive-sequence/):
  [💡](https://www.youtube.com/watch?v=P6RZZMu_maU)
  - `nums = [100,4,200,1,3,2]` => `4`
  - Set and start if n-1 not in set
  - O(n) time, O(n) space

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
  - `s = "A man, a plan, a canal: Panama"` => `true`
  - Two pointers, `if not s[i].isalnum(): i += 1`
  - O(n) time, O(1) space
- 👀[**3Sum**](https://leetcode.com/problems/3sum/):
  [💡](https://www.youtube.com/watch?v=jzZsG8n2R9A)
  - `nums = [-1,0,1,2,-1,-4]` => `[[-1,-1,2],[-1,0,1]]`
  - sort, 2 pointers
  - O(n^2) time, O(n) space (sorting)
- 👀[**Container With Most Water**](https://leetcode.com/problems/container-with-most-water/):
  [💡](https://www.youtube.com/watch?v=UuiTKBwPgAo)
  - `height = [1,8,6,2,5,4,8,3,7]` => `49`
  - start from both ends, move the smaller one
  - O(n) time, O(1) space

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
  - `prices = [7,1,5,3,6,4]` => `5`
  - [Kadane's algorithm](https://raw.githubusercontent.com/neetcode-gh/leetcode/main/python/121-Best-Time-To-Buy-and-Sell-Stock.py)
  - O(n) time, O(1) space
- 👀[**Longest Substring Without Repeating Characters**](https://leetcode.com/problems/longest-substring-without-repeating-characters/):
  [💡](https://www.youtube.com/watch?v=wiGpQwVHdE0)
  - `s = "abcabcbb"` => `3` ("abc")
  - Hashmap (seen, index), `ans = max(ans, i - start + 1)`, update start
  - O(n) time, O(1) space
- 👀[**Longest Repeating Character Replacement**](https://leetcode.com/problems/longest-repeating-character-replacement/):
  [💡](https://www.youtube.com/watch?v=gqXU1UyA8pk)
  - `s = "ABAB", k = 2` => `4` (replace both A with B)
  - Window with most repeating, shrink `if windowLen - max(count.values()) > k`
  - O(n) time, O(1) space
- 👀[**Minimum Window Substring**](https://leetcode.com/problems/minimum-window-substring/):
  [💡](https://www.youtube.com/watch?v=jSto0O4AJbM)
  - `s = "ADOBECODEBANC", t = "ABC"` => `"BANC"`
  - Counter, if all window counter >= counter, shrink
  - O(n) time, O(1) space
---

- ✅[**Maximum Points You Can Obtain from Cards**](https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/):
  [💡](https://www.youtube.com/watch?v=TsA4vbtfCvo)
  - `cardPoints = [1,2,3,4,5,6,1], k = 3` => `12`
  - Sliding window, minimum subarray of size n-k.
  - O(n) time, O(1) space
- ✅[**Subarray Sum Equals K**](https://leetcode.com/problems/subarray-sum-equals-k/):
  [💡](https://youtu.be/fFVZt-6sgyo)
  - `nums = [1,1,1], k = 2` => `2` ([1, 1] and [1, 1])
  - prefix sum, if increase by k, found (subarray in between), initialize `d[0] = 1`
  - O(n) time, O(n) space

## Stack

---

- 👀[**Valid Parentheses**](https://leetcode.com/problems/valid-parentheses/):
  [💡](https://www.youtube.com/watch?v=WTzjTskDFMg)
  - `s = "()[]{}"` => `true`
  - store last opened in stack
  - O(n) time, O(n) space

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

- 👀[**Search in Rotated Sorted Array**](https://leetcode.com/problems/search-in-rotated-sorted-array/):
  [💡](https://www.youtube.com/watch?v=U8XENwh8Oy8)
  - `nums = [4,5,6,7,0,1,2], target = 0` => `4`
  - see which side is sorted and then `if target > nums[mid] or target < nums[l]`
  - O(logn) time, O(1) space
- 👀[**Find Minimum in Rotated Sorted Array**](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/):
  [💡](https://www.youtube.com/watch?v=nIVW4P8b1VA)
  - `nums = [3,4,5,1,2]` => `1`
  - stop `if nums[l] < nums[r]`, move `l` if `nums[mid] >= nums[l]`
  - O(logn) time, O(1) space

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

- 👀[**Merge Two Sorted Lists**](https://leetcode.com/problems/merge-two-sorted-lists/):
  [💡](https://www.youtube.com/watch?v=XIdigk956u0)
  - `l1 = [1,2,4], l2 = [1,3,4]` => `[1,1,2,3,4,4]`
  - dummy first node, curr node, while l1 and l2, add remaining
  - O(n) time, O(1) space
- 👀[**Merge K Sorted Lists**](https://leetcode.com/problems/merge-k-sorted-lists/):
  [💡](https://www.youtube.com/watch?v=q5a5OiGbT6Q)
  - `lists = [[1,4,5],[1,3,4],[2,6]]` => `[1,1,2,3,4,4,5,6]`
  - Merge two lists at a time.
  - O(nlogk) time, O(1) space
- 👀[**Remove Nth Node From End of List**](https://leetcode.com/problems/remove-nth-node-from-end-of-list/):
  [💡](https://www.youtube.com/watch?v=XVuQxVej6y8)
  - `head = [1,2,3,4,5], n = 2` => `[1,2,3,5]`
  - Two pointers, move one n steps ahead.
  - O(n) time, O(1) space
- 👀[**Reverse Linked List**](https://leetcode.com/problems/reverse-linked-list/):
  [💡](https://www.youtube.com/watch?v=G0_I-ZF0S38)
  - `head = [1,2]` => `[2,1]`
  - while cur: ..., tmp, update prev and cur, return prev
  - O(n) time, O(1) space
- 👀[**Linked List Cycle**](https://leetcode.com/problems/linked-list-cycle/):
  [💡](https://www.youtube.com/watch?v=gBTe7lFR3vc)
  - `head = [3,2,0,-4], -4 -> 2` => `true`
  - slow = fast = head, while fast and fast.next:
  - O(n) time, O(1) space
- 👀[**Reorder List**](https://leetcode.com/problems/reorder-list/)
  [💡](https://www.youtube.com/watch?v=S5bfdUTrKLM)
  - `head = [1,2,3,4]` => `[1,4,2,3]` (L0 -> Ln -> L1 -> Ln-1 -> L2...)
  - find mid, reverse second half, merge two lists
  - O(n) time, O(1) space

---

- ✅[**Middle of the Linked List**](https://leetcode.com/problems/middle-of-the-linked-list/):
  [💡]()
  - `1->2->3->4->5` => `3`
  - slow = fast = head, while fast and fast.next: ..., return slow
  - O(n) time, O(1) space
- ✅[**Linked List Cycle II**](https://leetcode.com/problems/linked-list-cycle-ii/):
  [💡](https://leetcode.com/problems/linked-list-cycle-ii/solutions/1701128/c-java-python-slow-and-fast-image-explanation-beginner-friendly/?orderBy=most_votes)
  - `head = [3,2,0,-4], -4 -> 2` => `2`
  - dist(intersect, cycle) == dist(head, cycle)
  - O(n) time, O(1) space
## Trees

---

Careful with recursion limit (bound to the application stack)

- 👀[**Invert Binary Tree**](https://leetcode.com/problems/invert-binary-tree/):
  [💡](https://leetcode.com/problems/invert-binary-tree/solutions/62705/python-solutions-recursively-dfs-bfs/?orderBy=most_votes)
  - `root = [4,2,7,1,3,6,9]` => `[4,7,2,9,6,3,1]`
  - `r.right, r.left = self.invert(r.left), self.invert(r.right)` or stack
  - O(n) time, O(height) space
- 👀[**Maximum Depth of Binary Tree**](https://leetcode.com/problems/maximum-depth-of-binary-tree/):
  [💡](https://www.youtube.com/watch?v=hTM3phVI6YQ)
  - `root = [3,9,20,null,null,15,7]` => `3`
  - `return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1`
  -  O(n) time, O(height) space
- 👀[**Same Tree**](https://leetcode.com/problems/same-tree/):
  [💡](https://www.youtube.com/watch?v=vRbbcKXCxOw)
  - `p = [1,2,3], q = [1,2,3]` => `true`
  - `isSameTree(p.left, q.left) and isSameTree(p.right, q.right)`
  - O(n) time, O(height) space
- 👀[**Subtree of Another Tree**](https://leetcode.com/problems/subtree-of-another-tree/):
  [💡](https://www.youtube.com/watch?v=E36O5SWp-LE)
  - `s = [3,4,5,1,2], t = [4,1,2]` => `true`
  - `isSameTree(s, t) or isSubtree(s.left, t) or isSubtree(s.right, t)`
  - O(nm) time, O(n+m) space
- 👀[**Binary Tree Level Order Traversal**](https://leetcode.com/problems/binary-tree-level-order-traversal/):
  [💡](https://leetcode.com/problems/binary-tree-level-order-traversal/solutions/33464/5-6-lines-fast-python-solution-48-ms/)
  - `root = [3,9,20,null,null,15,7]` => `[[3],[9,20],[15,7]]`
  - `while level: ans.append([node.val for node in level]); level = [...]` or queue
  - O(n) time, O(n) space
- 👀[**Construct Binary Tree from Preorder and Inorder Traversal**](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/):
  [💡](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/solutions/34579/python-short-recursive-solution/)
  - `preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]` => `[3,9,20,null,null,15,7]`
  ```python
  root = TreeNode(preorder[0])
  i = inorder.index(root.val)
  root.left = self.buildTree(preorder[1:i+1], inorder[:i])
  root.right = self.buildTree(preorder[i+1:], inorder[i+1:])
  ```
  - O(n) time, O(n) space
- 👀[**Binary Tree Maximum Path Sum**](https://leetcode.com/problems/binary-tree-maximum-path-sum/):
  [💡](https://leetcode.com/problems/binary-tree-maximum-path-sum/solutions/603423/python-recursion-stack-thinking-process-diagram/)
  - `root = [1,2,3]` => `6`
  - dfs, allow split or not, nonlocal max
  - O(n) time, O(height) space
- 👀[**Serialize and Deserialize Binary Tree**](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/):
  [💡](https://www.youtube.com/watch?v=u4JAi2JJhI8)
  - `serialize(self, root)` and `deserialize(self, data)`
  - Preorder travelsal, `"1,2,N,N,3,4,N,N,5,N,N"`
  - O(n) time, O(n) space

> BST: nodes left < node < nodes right

- 👀[**Lowest Common Ancestor of a Binary Search Tree**](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/):
  [💡](https://www.youtube.com/watch?v=gs2LMfuOR9k)
  - `root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8` => `6`
  - cur, left `if p<cur and q<cur`, right `if p>cur and q>cur` else return
  - O(n) time, O(1) space
- 👀[**Validate Binary Search Tree**](https://leetcode.com/problems/validate-binary-search-tree/):
  [💡](https://www.youtube.com/watch?v=s6ATEkipzow)
  - `root = [2,1,3]` => `true`
  - `validate(n.left, min_val, n.val) and validate(n.right, n.val, max_val)`
  - O(n) time, O(height) space
- 👀[**Kth Smallest Element in a BST**](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)
  [💡](https://www.youtube.com/watch?v=5LUXSvjmGCw)
  - `root = [3,1,4,null,2], k = 1` => `1`
  - inorder traversal, return res[k-1] (or iterative inorder with stack)
  - O(n) time, O(height) space
---

- ✅[**Find Leaves of Binary Tree**](https://www.lintcode.com/problem/650/):
  [💡]
  - dfs, post-order, return layer.

## Tries

---

- 👀[**Implement Trie (Prefix Tree)**](https://leetcode.com/problems/implement-trie-prefix-tree/):
  [💡](https://www.youtube.com/watch?v=oobqoCJlHA0)
  - `insert(word)`, `search(word)`, `startsWith(prefix)`
  - node with dict[char, node] and a bool isWord.
  - O(n) time, O(n) space
- 👀[**Design Add And Search Words Data Structure**](https://leetcode.com/problems/design-add-and-search-words-data-structure/):
  [💡](https://www.youtube.com/watch?v=BTf05gs_8iU)
- 👀[**Word Search II**](https://leetcode.com/problems/word-search-ii/)
  [💡](https://www.youtube.com/watch?v=asbcE9mZz_U)

## Heap & Priority Queue

---

> Heap invariant: each node is <= than its children.

- 👀[**Find Median from Data Stream**](https://leetcode.com/problems/find-median-from-data-stream/)
  [💡](https://leetcode.com/problems/find-median-from-data-stream/solutions/74047/java-python-two-heap-solution-o-log-n-add-o-1-find/)
  - `addNum(num)` and `findMedian()`
  - 2 heaps, max heap for left, min heap for right, balance
  - O(logn) time, O(n) space
---

- ✅[**Stock Price Fluctuation**](https://leetcode.com/problems/stock-price-fluctuation/):
  [💡](https://leetcode.com/problems/stock-price-fluctuation/solutions/1513293/python-clean-2-heaps-commented-code/?orderBy=most_votes)
  - `update(int timestamp, int price)`, `current()`, `maximum()`, `minimum()`
  - 2 heaps, timestamps[time, price], self.highest_timestamp.
- [**Kth Largest Element in an Stream**](https://leetcode.com/problems/kth-largest-element-in-a-stream/):
  `while len(self.heap) > k: heapq.heappop(self.heap)`

## Backtracking

---

- 👀[**Combination Sum**](https://leetcode.com/problems/combination-sum/)
  [💡](https://www.youtube.com/watch?v=GBKI9VSKdGg)
  - `candidates = [2,3,6,7], target = 7` => `[[7],[2,2,3]]`
  - dfs, backtracking, append path to global res
  - O(2^target) time, O(target) space
- 👀[**Word Search**](https://leetcode.com/problems/word-search/)
  [💡](https://www.youtube.com/watch?v=pfiQ_PS1g8E)
  - `board = [["A","B","C","E"],["S","F","C","S"],...], word = "ABCCED"` => `true`
  - dfs, backtracking, mark visited
  - O(mn*4^l) time, O(l) space
---

- [**Sudoku Solver**](https://leetcode.com/problems/sudoku-solver/): `if board[3 * (i // 3) + k // 3][ 3 * (j // 3) + k % 3] == n:`

## Graphs

---

- 👀[**Number of Islands**](https://leetcode.com/problems/number-of-islands/):
  [💡](https://www.youtube.com/watch?v=pV2kpPD66nE)
  - `grid = [["1", "0"], ["0", "1"]]` => `2`
  - skip visited (mark 0 or set), dfs (4 directions) on 1s.
  - O(cells) time, O(cells) space
- 👀[**Clone Graph**](https://leetcode.com/problems/clone-graph/):
  [💡](https://www.youtube.com/watch?v=mQeF6bN8hMk)
  - Node with val, neighbors.
  - cache[old] = copy, for n in neighbors: copy.neighbors.append(dfs(n))).
  - O(v+e) time, O(v) space
- 👀[**Pacific Atlantic Water Flow**](https://leetcode.com/problems/pacific-atlantic-water-flow/):
  [💡](https://www.youtube.com/watch?v=s-VkcjHqkGI)
  - grid of heights.
  - start from edge and dfs(x, y, ocean, last_height) to higher, return set intersection
  - O(mn) time, O(mn) space
- 👀[**Number of Connected Components in an Undirected Graph**](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/):
  [💡](https://www.youtube.com/watch?v=8f1XPm4WOUc)
  - `n = 5, edges = [[0, 1], [1, 2], [3, 4]]` => `2`
  - union find
  - ???
- 👀[**Graph Valid Tree**](https://www.lintcode.com/problem/graph-valid-tree/description):
  [💡](https://www.youtube.com/watch?v=bXsUuownnoQ)
  - `n = 5, edges = [[0, 1], [0, 2], [0, 3], [1, 4]]` => `true`
  - adj list, DFS cycle detection, `dfs(0, prev=-1) and n == len(visit)`
  - O(e+v) time, O(e+v) space
## Topological Sort

---

- 👀[**Course Schedule**](https://leetcode.com/problems/course-schedule/):
  [💡](https://www.youtube.com/watch?v=EgI5nU9etnU)
  - BFS topological sort / DFS cycle detection (visited set, graph[c] = []).
- 👀[**Alien Dictionary**](https://www.lintcode.com/problem/892):
  [💡](https://www.youtube.com/watch?v=6kTZYvNNyps)
  - topological sort, DFS cycle detection.

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

https://youtu.be/_i4Yxeh5ceQ

- 👀[**Coin Change**](https://leetcode.com/problems/coin-change/): 
  [💡](https://www.youtube.com/watch?v=H9bfqozjoqs) Unbounded Knapsack
  - `coins = [1,2,5], amount = 11` => `3`
  - Cache and iterate `range(amount+1)`, recursive is O(coins^amount)
  - O(coins\*amount) time, O(amount) space
- 👀[**Climbing Stairs**](https://leetcode.com/problems/climbing-stairs/):
  [💡](https://www.youtube.com/watch?v=Y0lT9Fck7qI) Fibonacci
  - `n = 2` => `2`, `n = 3` => `3`
  - `temp = n1 + n2`
  - O(n) time, O(1) space
- 👀[**House Robber**](https://leetcode.com/problems/house-robber/):
  [💡](https://leetcode.com/problems/house-robber/solutions/1605797/c-python-4-simple-solutions-w-explanation-optimization-from-brute-force-to-dp/)
  - `nums = [2,7,9,3,1]` => `12` (2, 9, 1)
  - `rob1, rob2 = 0, 0`, `tmp = max(rob1 + n, rob2)`, `return rob2`
  - O(n) time, O(1) space
- 👀[**House Robber II**](https://leetcode.com/problems/house-robber-ii/):
  [💡](https://www.youtube.com/watch?v=rWAJCfYYOvM)
  - `nums = [2,3,2]` => `3` (circular)
  - `max(nums[0], rob1(nums[1:]), rob1(nums[:-1]))`
  - O(n) time, O(1) space
- 👀[**Longest Palindromic Substring**](https://leetcode.com/problems/longest-palindromic-substring/):
  [💡](https://www.youtube.com/watch?v=XYQecbcd6_c)
  - `s = "babad"` => `"bab"` or `"aba"`
  - for i in range(len(s)): expand around l, r = 1, 1 and l, r = i, i+1
  - O(n^2) time, O(1) space
- 👀[**Palindrome Substrings**](https://leetcode.com/problems/palindromic-substrings/):
  [💡](https://www.youtube.com/watch?v=4RACzI5-du8)
  - `s = "abc"` => `3` (a, b, c)
  - similar to the preview one, expand and add to count
  - O(n^2) time, O(1) space
- 👀[**Maximum Product Subarray**](https://leetcode.com/problems/maximum-product-subarray/):
  [💡](https://www.youtube.com/watch?v=lXVy6YWFcRM)
  - `nums = [2,3,-2,4]` => `6` (2, 3)
  - `cur_max, cur_min, max_prod = 1, 1, float('-inf')`, reset if n == 0
  - O(n) time, O(1) space
- 👀[**Decode Ways**](https://leetcode.com/problems/decode-ways/):
  [💡](https://www.youtube.com/watch?v=6aEyTjOwlJU)
  - `s = "12"` => `2` ("AB" or "L")
  - start from end, `dp[i] = dp[i+1]` add `dp[i+2]` if `dp[i:i+2]` is valid
  - O(n) time, O(n) space
- 👀[**Word Break**](https://leetcode.com/problems/word-break/):
  [💡](https://www.youtube.com/watch?v=Sx9NNgInc3A)
  - `s = "leetcode", words = ["leet", "code"]` => `true`
  - start from end, `dp[i] = any(dp[i+len(w)] for w in words if s[i:i+len(w)] == w)`
  - O(n^2) time, O(n) space
- 👀[**Longest Increasing Subsequence**](https://leetcode.com/problems/longest-increasing-subsequence/):
  [💡](https://www.youtube.com/watch?v=cjWnW0hdF1Y)
  - `nums = [10,9,2,5,3,7,101,18]` => `4` (2, 3, 7, 101) strict
  - start from end, `if nums[i] < nums[j]: dp[i] = max(dp[i], dp[j] + 1)`
  - O(n^2) time, O(n) space
---

- [**Min cost climbing stairs**](https://leetcode.com/problems/min-cost-climbing-stairs/):
  [💡](https://youtu.be/ktmzAZWkEZ0)
  - `cost = [10, 15, 20]` => `15` (start from 0 or 1, 1 or 2 steps)
  - `new = min(prev1+c[i-2], prev2+c[i-1]); prev1 = prev2; prev2 = new`
  - O(n) time, O(1) space
- ✅[**Student Attendance Record II**](https://leetcode.com/problems/student-attendance-record-ii/):
  Fewer than 2 A, no 3 or more consecutive L.
  [💡]
  - `n = 2` => `8` ("PP", "AP", "PA", "LP", "PL", "AL", "LA", "LL")

### 2D

- 👀[**Unique Paths**](https://leetcode.com/problems/unique-paths/)
  [💡](https://www.youtube.com/watch?v=IlEsdxuD4lY)
  - `m = 3, n = 2` => `3`
  - `cache[0, 0] = 1; cache[i, j] = cache.get((i-1, j), 0) + cache.get((i, j-1), 0)`
  - O(m\*n) time, O(m\*n) space
- 👀[**Longest Common Subsequence**](https://leetcode.com/problems/longest-common-subsequence/)
  [💡](https://www.youtube.com/watch?v=Ua0GhsJSlWM)
  - `text1 = "abcde", text2 = "ace"` => `3` ("ace")
  - dp/recursive, add one and increase both if equal, else get the max of i+1 and j+1
  - O(m\*n) time, O(m\*n) space (can be reduced)
---

- ✅[**Coin Change 2**](https://leetcode.com/problems/coin-change-2/):
  [💡](https://www.youtube.com/watch?v=Mjy4hd2xgrs)
  - `amount = 5, coins = [1, 2, 5]` => `4`
  - Unbounded Knapsack
- ✅[**Target Sum**](https://leetcode.com/problems/target-sum/):
  [💡](https://www.youtube.com/watch?v=g0npyaQtAQM)
  - assign + or - `nums = [1, 1, 1, 1, 1], target = 3` => `5`
  - 0/1 Knapsack, O(2^n) -> O(n\*sum(nums)))
- ✅[**Edit Distance**](https://leetcode.com/problems/edit-distance/):
  [💡](https://leetcode.com/problems/edit-distance/solutions/159295/python-solutions-and-intuition/?orderBy=most_votes)
  - `word1 = "horse", word2 = "ros"` => `3`
  - lru_cache, `dp(len(s1), len(s2))`
  - O(m\*n) time, O(m\*n) space

## Greedy

---

- 👀[**Maximum Subarray**](https://leetcode.com/problems/maximum-subarray/):
  [💡](https://www.youtube.com/watch?v=5WZl3MMT0Eg)
  - `nums = [-2,1,-3,4,-1,2,1,-5,4]` => `6` (`[4,-1,2,1]`)
  - `current_sum = max(current_sum+n, n)`, Kadane.
  - O(n) time, O(1) space
- 👀[**Jump Game**](https://leetcode.com/problems/jump-game/):
  [💡](https://www.youtube.com/watch?v=Yan0cv2cLy8)
  - `nums = [2,3,1,1,4]` => `true` (can reach last index)
  - start from end, `if i + nums[i] >= target: target = i`.
  - O(n) time, O(1) space
## Intervals

---

- 👀[**Merge Intervals**](https://leetcode.com/problems/merge-intervals/):
  [💡](https://www.youtube.com/watch?v=44H3cEC2fFM)
  - `intervals = [[1,3],[2,6],[8,10],[15,18]]` => `[[1,6],[8,10],[15,18]]`
  - Sort by start, append to result and merge `if start <= result[-1][1]`
  - O(nlogn) time, O(n) space
- 👀[**Meeting Rooms II**](https://www.lintcode.com/problem/919/):
  [💡](https://www.youtube.com/watch?v=FdzJmTCVyJU)
  min number of conference rooms.
  - `intervals = [(0,30),(5,10),(15,20)]` => `2`
  - sorted starts and ends, s and e pointers, increment count `if start[s] < end[e]`
  - O(nlogn) time, O(n) space
- 👀[**Insert Interval**](https://leetcode.com/problems/insert-interval/):
  [💡](https://www.youtube.com/watch?v=A8NUOmlwOlM)
  - `intervals = [[1,3],[6,9]], newInterval = [2,5]` => `[[1,5],[6,9]]` (sorted)
  - `if newInterval[1] < interval[0]`, `if newInterval[0] > interval[1]`, else merge
  - O(n) time, O(n) space
- 👀[**Non-overlapping Intervals**](https://leetcode.com/problems/non-overlapping-intervals/):
  [💡](https://www.youtube.com/watch?v=nONCGxWoUfM)
  - `intervals = [[1,2],[2,3],[3,4],[1,3]]` => `1` (number of intervals to remove)
  - sort, update if start >= prev.end, else increase count
  - O(nlogn) time, O(1) space
- 👀[**Meeting Rooms**](https://www.lintcode.com/problem/920/):
  [💡](https://www.youtube.com/watch?v=PaJxqZVPhbg)
  - `intervals = [(0,30),(5,10),(15,20)]` => `false`
  - sort, if start < prev.end, return false
  - O(nlogn) time, O(n) space

## Math & Geometry

---

- 👀[**Rotate Image**](https://leetcode.com/problems/rotate-image/):
  [💡](https://www.youtube.com/watch?v=fMSJSS7eO1w)
  - `matrix = [[1,2,3],[4,5,6],[7,8,9]]` => `[[7,4,1],[8,5,2],[9,6,3]]`
  - while l < r, swap 4 corners, move inwards
  - O(n^2) time, O(1) space
- 👀[**Spiral Matrix**](https://leetcode.com/problems/spiral-matrix/):
  [💡](https://www.youtube.com/watch?v=BJnMZNwUk1M)
  - `matrix = [[1,2,3],[4,5,6],[7,8,9]]` => `[1,2,3,6,9,8,7,4,5]`
  - 4 pointers, left, right, top, bottom, while l <= r and t <= b
  - O(n^2) time, O(1) space
- 👀[**Set Matrix Zeroes**](https://leetcode.com/problems/set-matrix-zeroes/):
  [💡](https://www.youtube.com/watch?v=T41rL0L3Pnw)
  - `matrix = [[1,1,1],[1,0,1],[1,1,1]]` => `[[1,0,1],[0,0,0],[1,0,1]]`
  - use first row and column to store if row or column should be zeroed
  - O(mn) time, O(1) space
---

- ✅[**Happy Number**](https://leetcode.com/problems/happy-number/):
  [💡]
  - `n = 19` => `true` (`12 + 92 = 82 |...| 12 + 02 + 02 = 1`)
  - Hashset seen or Floyd's cycle detection

## Bit Manipulation (rare)

---

[Summary](https://leetcode.com/problems/sum-of-two-integers/discuss/84278/A-summary%3A-how-to-use-bit-manipulation-to-solve-problems-easily-and-efficiently)

- 👀[**Number of 1 Bits**](https://leetcode.com/problems/number-of-1-bits/):
  [💡](https://www.youtube.com/watch?v=5Km3utixwZs)
  - `n = 11` => `3` (1011)
  - either `res += n & 1` and `n >> 1` or just `n = n & (n-1)` and `res += 1`
  - O(logn) time, O(1) space
- 👀[**Counting Bits**](https://leetcode.com/problems/counting-bits/):
  [💡](https://www.youtube.com/watch?v=RyBM56RIWrM)
  - `n = 2` => `[0,1,1]`
  - `if offset * 2 == i: offset *= i`, `dp[i] = dp[i-offset] + 1`
  - O(n) time, O(n) space
- 👀[**Reverse Bits**](https://leetcode.com/problems/reverse-bits/):
  [💡](https://www.youtube.com/watch?v=UcoN6UjAI64)
  - `00000000000000000000000000000110` => `01100000000000000000000000000000`
  - `for i in range(32): bit = (n >> i) & 1`, `res |= bit << (31-i)`
  - O(1) time, O(1) space (32 bit)
- 👀[**Missing Number**](https://leetcode.com/problems/missing-number/):
  [💡](https://www.youtube.com/watch?v=WnPLSRLSANE)
  - `nums = [3,0,1]` => `2`
  - `for i, n in enumerate(nums): res ^= n; res ^= i+1`
  - O(n) time, O(1) space
- 👀[**Sum of Two Integers**](https://leetcode.com/problems/sum-of-two-integers/):
  [💡](https://www.youtube.com/watch?v=gVUrDV4tZfY)
  - `a = 1, b = 2` => `3`
  - `return a if b == 0 else getSum(a^b, (a&b)<<1)`

## Extra

---

- [**Network Delay Time**](https://leetcode.com/problems/network-delay-time/):
  Dijkstra.
- [**Path with Maximum Probability**](https://leetcode.com/problems/path-with-maximum-probability/):
  [💡](https://leetcode.com/problems/path-with-maximum-probability/solutions/731767/java-python-3-2-codes-bellman-ford-and-dijkstra-s-algorithm-w-brief-explanation-and-analysis/)
  - Dijkstra.
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
