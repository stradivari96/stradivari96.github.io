---
title: "📝 Data structures and algorithms"
date: 2022-10-27
searchHidden: true
robotsNoIndex: true
---

Some notes about DSA

<!--more-->

<script>
    function openRandomLink(neetcode, tag) {
        var links = document.querySelectorAll("a");
        links = Array.from(links)
        links = links.filter(a => a.href.startsWith("https://leetcode.com/problems/") && !a.href.includes("solutions"));
        var limit = neetcode ? 150 : links.length;
        var randomLink = links[Math.floor(Math.random() * limit)];
        while (tag && !randomLink.href.includes(tag)) {
            randomLink = links[Math.floor(Math.random() * limit)];
        }
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

https://leetcode.com/assessment/

<button onclick="openRandomLink(true, '?ez')" style="border: solid; border-width:1px; margin: 5px; padding: 5px" >
Neetcode ez
</button>
<button onclick="openRandomLink(true, '?md')" style="border: solid; border-width:1px; margin: 5px; padding: 5px" >
Neetcode md
</button>
<button onclick="openRandomLink(true, '?hd')" style="border: solid; border-width:1px; margin: 5px; padding: 5px" >
Neetcode hard
</button>
<br/>
<button onclick="openRandomLink(false, '?md')" style="border: solid; border-width:1px; margin: 5px; padding: 5px" >
Random md
</button>

### Big O

- [Neetcode](https://youtu.be/BgLTDT03QtU)
- [Big O Cheat Sheet](https://www.bigocheatsheet.com/)
- Recursive with multiple branches: O(branches^depth), if not repeated work, O(n).
- Fibonacci: O(2^n), with memoization: O(n)
- Permutations, TSP: O(n!)
- Binary search: O(log n) (divide search space)

### Legend

- 🅱️ Blind 75 / 🗿 Neetcode / 🏢 Google
- ☀️ Easy / ⛅ Medium / ⛈️ Hard

## Arrays

- [Kadane's algorithm](https://neetcode.io/courses/advanced-algorithms/0):
  grow and check when to shrink

```python
for n in nums:
    # need to shrink?
    if cur_sum+n < n: ...
    # update result
    if cur_sum > max_sum: ...
```

- [Sliding window fixed size](https://neetcode.io/courses/advanced-algorithms/1):
  l, r pointers and current window set

```python
for r in range(len(nums)):
    if r-l+1 == k:
        ...
```

- [Sliding window variable size](https://neetcode.io/courses/advanced-algorithms/2):
  l, r pointers, iterate over r, grow while possible and shrink
- [Two pointers](https://neetcode.io/courses/advanced-algorithms/3):
  Palindrome, l, r from ends, choose which to move
- [Prefix Sums](https://neetcode.io/courses/advanced-algorithms/4):
  Get sum of any subarray in O(1), prefix / suffix products, etc

---

### Basic & Hashing

- 🅱️[**Contains Duplicate**](https://leetcode.com/problems/contains-duplicate/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=3OamzN90kPg)

  - `nums = [1,2,3,1]` => `true`
  - Hashmap seen / sort
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def containsDuplicate(self, nums: List[int]) -> bool:
        """O(n) time, O(n) space"""
        seen = set()

        for n in nums:
            if n in seen:
                return True
            seen.add(n)
        return False

    def containsDuplicate(self, nums: List[int]) -> bool:
        """O(nlogn) time, O(1) space"""
        nums.sort()
        for i in range(1, len(nums)):
            if nums[i] == nums[i - 1]:
                return True
        return False
    ```

    </details>

- 🅱️[**Valid Anagram**](https://leetcode.com/problems/valid-anagram/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=9UtInBqnCgA)

  - `s = "anagram", t = "nagaram"` => `true`
  - Counter for each string
  - <details>
      <summary>O(w) time, O(1) space (26 chars)</summary>

    ```python
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
        count_s = Counter(s)
        count_t = Counter(t)
        return count_s == count_t
    ```

    </details>

- 🅱️🏢[**Two Sum**](https://leetcode.com/problems/two-sum/?ez)☀️:
  [💡](https://youtu.be/KLlXCFG5TnA)

  - `nums = [2,7,11,15], target = 9` => `[0,1]`
  - Hashmap seen with index
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        seen = {}
        for i, n in enumerate(nums):
            if target - n in seen:
                return seen[target - n], i
            seen[n] = i
    ```

    </details>

- 🅱️[**Group Anagrams**](https://leetcode.com/problems/group-anagrams/?md)⛅:
  [💡](https://www.youtube.com/watch?v=vzdNOK2oB2E)

  - `strs = ["tea","tan","ate","nat"]` => `[["nat","tan"],["ate","tea"]]`
  - Counter for each string, use tuple as key
  - <details>
      <summary>O(strings*average_lenth) time, O(strings) space</summary>

    ```python
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        groups = defaultdict(list)
        for word in strs:
            groups[tuple(sorted(Counter(word).items()))].append(word)
        return list(groups.values())
    ```

    </details>

- 🅱️[**Top K Frequent Elements**](https://leetcode.com/problems/top-k-frequent-elements/?md)⛅:
  [💡](https://www.youtube.com/watch?v=YPTqKIgVk-k)

  - `nums = [1,1,1,2,2,3], k = 2` => `[1,2]`
  - Counter, list of frequencies (len(nums)), iterate from end
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        groups_by_freq = [[] for _ in range(len(nums)+1)]
        for n, count in Counter(nums).items():
            groups_by_freq[count].append(n)
        result = []
        for i in range(len(nums), -1, -1):
            for n in groups_by_freq[i]:
                result.append(n)
                if len(result) >= k:
                    return result
        return result
    ```

    </details>

- 🅱️[**Product of Array Except Self**](https://leetcode.com/problems/product-of-array-except-self/?md)⛅:
  [💡](https://www.youtube.com/watch?v=bNvIQI2wAjk)

  - `nums = [1,2,3,4]` => `[24,12,8,6]`
  - Prefix product, Suffix product.
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def productExceptSelf(self, nums):
        ans, suf, pre = [1]*len(nums), 1, 1
        for i in range(len(nums)):
            ans[i] *= pre               # prefix product from one end
            pre *= nums[i]
            ans[-1-i] *= suf            # suffix product from other end
            suf *= nums[-1-i]
        return ans
    ```

    </details>

- 🗿[**Valid Sudoku**](https://leetcode.com/problems/valid-sudoku/?md)⛅:
  [💡](https://www.youtube.com/watch?v=TjFXEUCMqI8)

  - partially filled 9x9 grid
  - sets, `squares[(r // 3, c // 3)].add(board[r][c])`
  - <details>
      <summary>O(1) time, O(1) space</summary>

    ```python
    def isValidSudoku(self, board: List[List[str]]) -> bool:

        for row in board:
            numbers = [n for n in row if n != "."]
            if len(set(numbers)) != len(numbers):
                return False

        for i in range(9):
            numbers = [row[i] for row in board if row[i] != "."]
            if len(set(numbers)) != len(numbers):
                return False

        squares = defaultdict(set)
        for r in range(9):
            for c in range(9):
                block = squares[(r // 3, c // 3)]
                if board[r][c] != "." and board[r][c] in block:
                    return False
                block.add(board[r][c])
        return True
    ```

    </details>

- 🅱️[**Encode and Decode Strings**](https://leetcode.com/problems/encode-and-decode-strings/?md)⛅:
  [💡](https://www.youtube.com/watch?v=B1k_sxOSgv8)

  - Create single string and split it back
  - Length + Separator
  - <details>
      <summary>O(n) encode, O(n) decode</summary>

    ```python
    def encode(self, strs):
        res = ""
        for s in strs:
            res += str(len(s)) + "#" + s
        return res

    def decode(self, s):
        res, i = [], 0

        while i < len(s):
            j = i
            while s[j] != "#":
                j += 1
            length = int(s[i:j])
            res.append(s[j + 1 : j + 1 + length])
            i = j + 1 + length
        return res
    ```

    </details>

- 🅱️[**Longest Consecutive Sequence**](https://leetcode.com/problems/longest-consecutive-sequence/?md)⛅:
  [💡](https://www.youtube.com/watch?v=P6RZZMu_maU)

  - `nums = [100,4,200,1,3,2]` => `4`
  - Set and start if n-1 not in set
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def longestConsecutive(self, nums: List[int]) -> int:
        if not nums:
            return 0
        hashset = set(nums)
        result = 1
        for n in hashset:
            if (n-1) not in hashset:
                length = 0
                cur = n
                while cur in hashset:
                    cur = cur+1
                    length += 1
                result = max(result, length)
        return result
    ```

    </details>

### Two Pointers

- 🅱️[**Valid Palindrome**](https://leetcode.com/problems/valid-palindrome/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=jJXJ16kPFWg)

  - `s = "A man, a plan, a canal: Panama"` => `true`
  - Two pointers, `if not s[i].isalnum(): i += 1`
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def isPalindrome(self, s: str) -> bool:
        l = 0
        r = len(s)-1
        while l < r:
            while l < len(s) and not s[l].isalnum(): l += 1
            while r >= 0 and not s[r].isalnum(): r -= 1
            if l >= r:
                return True
            if s[l].lower() != s[r].lower():
                return False
            l += 1
            r -= 1

        return True
    ```

    </details>

- 🗿[**Two Sum II**](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/?md)⛅:
  [💡](https://www.youtube.com/watch?v=cQ1Oz4ckceM)

  - `numbers = [2,7,11,15], target = 9` => `[1,2]`
  - 2 pointers.
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        l = 0
        r = len(numbers)-1
        while l < r:
            cursum = numbers[l] + numbers[r]
            if cursum == target:
                return [l+1, r+1]
            if cursum < target:
                l += 1
            else:
                r -= 1
        return None
    ```

    </details>

- 🅱️[**3Sum**](https://leetcode.com/problems/3sum/?md)⛅:
  [💡](https://www.youtube.com/watch?v=jzZsG8n2R9A)

  - `nums = [-1,0,1,2,-1,-4]` => `[[-1,-1,2],[-1,0,1]]`
  - sort, 2 pointers
  - <details>
      <summary>O(n^2) time, O(n) space (sorting)</summary>

    ```python
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        result = set()
        nums.sort()
        for i in range(len(nums)):
            l, r = i+1, len(nums)-1
            while l < r:
                sum3 = nums[l] + nums[r] + nums[i]
                if sum3 == 0:
                    result.add((nums[i], nums[l], nums[r]))
                    l += 1
                    r -= 1
                elif sum3 < 0:
                    l += 1
                else:
                    r -= 1
        return result
    ```

    </details>

- 🅱️[**Container With Most Water**](https://leetcode.com/problems/container-with-most-water/?md)⛅:
  [💡](https://www.youtube.com/watch?v=UuiTKBwPgAo)

  - `height = [1,8,6,2,5,4,8,3,7]` => `49`
  - start from both ends, move the smaller one
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def maxArea(self, height: List[int]) -> int:
        result = 0
        l, r = 0, len(height)-1
        while l < r:
            result = max(result, min(height[l], height[r])*(r-l))
            if height[l] < height[r]:
                l += 1
            else:
                r -= 1
        return result
    ```

    </details>

- 🗿[**Trapping Rain Water**](https://leetcode.com/problems/trapping-rain-water/?md)⛈️:
  [💡](https://www.youtube.com/watch?v=ZI2z5pq0TqA)

  - `height = [0,1,0,2,1,0,1,3,2,1,2,1]` => `6`
  - DP, can optimize to O(1) space by using 2 pointers (move `l` if leftmax < rightmax)
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def trap(self, height: List[int]) -> int:
        leftmax, rightmax = 0, 0
        l, r = 0, len(height)-1
        result = 0

        while l <= r:
            if leftmax <= rightmax:
                result += max(min(leftmax, rightmax)-height[l], 0)
                leftmax = max(leftmax, height[l])
                l += 1
            else:
                result += max(min(leftmax, rightmax)-height[r], 0)
                rightmax = max(rightmax, height[r])
                r -= 1
        return result
    ```

    </details>

### Sliding Window

- 🅱️🏢[**Best Time to Buy and Sell Stock**](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=1pkOgXD63yU)

  - `prices = [7,1,5,3,6,4]` => `5`
  - [Kadane's algorithm](https://raw.githubusercontent.com/neetcode-gh/leetcode/main/python/0121-best-time-to-buy-and-sell-stock.py) / Two pointers (start from 0, 1)
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def maxProfit(self, prices: List[int]) -> int:
        result = 0
        cheapest = prices[0]
        for p in prices:
            result = max(result, p-cheapest)
            cheapest = min(cheapest, p)
        return result
    ```

    </details>

- 🅱️🏢[**Longest Substring Without Repeating Characters**](https://leetcode.com/problems/longest-substring-without-repeating-characters/?md)⛅:
  [💡](https://www.youtube.com/watch?v=wiGpQwVHdE0)

  - `s = "abcabcbb"` => `3` ("abc")
  - Hashmap (seen, index), `ans = max(ans, i - start + 1)`, update start if < index+1
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def lengthOfLongestSubstring(self, s: str) -> int:
        if not s:
            return 0

        seen_idx = {}
        result = 1
        start = 0
        for idx, char in enumerate(s):
            if char in seen_idx and start <= seen_idx[char]+1:
                start = seen_idx[char]+1
            seen_idx[char] = idx
            result = max(result, idx-start+1)
        return result
    ```

    </details>

- 🅱️[**Longest Repeating Character Replacement**](https://leetcode.com/problems/longest-repeating-character-replacement/?md)⛅:
  [💡](https://www.youtube.com/watch?v=gqXU1UyA8pk)

  - `s = "ABAB", k = 2` => `4` (replace both A with B)
  - Window with most repeating, shrink `if windowLen - max(count.values()) > k`
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def characterReplacement(self, s: str, k: int) -> int:
        result = 0
        l = 0
        char_count = {}
        for r, c in enumerate(s):
            char_count[c] = char_count.get(c, 0) + 1
            if (r-l+1) > max(char_count.values())+k:
                char_count[s[l]] -= 1
                l += 1
            result = max(result, r-l+1)
        return result
    ```

    </details>

- 🗿[**Permutation in String**](https://leetcode.com/problems/permutation-in-string/?md)⛅:
  [💡](https://www.youtube.com/watch?v=UbyhOgBN834)

  - `s1 = "ab" s2 = "eidbaooo"` => `true`
  - Counter, sliding window of size len(s1)
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def checkInclusion(self, s1: str, s2: str) -> bool:
        cntr, w = Counter(s1), len(s1)
        for i in range(len(s2)):
            if s2[i] in cntr:
                cntr[s2[i]] -= 1
            if i >= w and s2[i-w] in cntr:
                cntr[s2[i-w]] += 1
            if all([cntr[i] == 0 for i in cntr]):
                return True
        return False
    ```

    </details>

- 🅱️[**Minimum Window Substring**](https://leetcode.com/problems/minimum-window-substring/?md)⛈️:
  [💡](https://www.youtube.com/watch?v=jSto0O4AJbM)

  - `s = "ADOBECODEBANC", t = "ABC"` => `"BANC"`
  - Counter, if all window counter >= counter, shrink
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def minWindow(self, s: str, t: str) -> str:
        need = collections.Counter(t)
        i = 0
        result = ""
        for j, c in enumerate(s, 1):
            need[c] -= 1
            if all(v <= 0 for v in need.values()):
                while i < j and need[s[i]] < 0:
                    need[s[i]] += 1
                    i += 1
                if not result or j-i < len(result):
                    result = s[i:j]
        return result
    ```

    </details>

- 🗿[**Sliding Window Maximum**](https://leetcode.com/problems/sliding-window-maximum/?md)⛈️:
  [💡](https://www.youtube.com/watch?v=DfljaUwZsOk)

  - `nums = [1,3,-1,-3,5,3,6,7], k = 3` => `[3,3,5,5,6,7]`
  - l, r pointers, Monotonic dec deque, store idx, pop everything smaller than current
  - <details>
      <summary>O(n) time, O(k) space</summary>

    ```python
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        result = []
        q = deque()
        l, r = 0, 0
        while r < len(nums):
            while q and nums[q[-1]] < nums[r]:
                q.pop()
            q.append(r)

            if l > q[0]:
                q.popleft()

            if r >= k-1:
                result.append(nums[q[0]])
                l += 1
            r += 1
        return result
    ```

    </details>

## Stack

---

- 🅱️[**Valid Parentheses**](https://leetcode.com/problems/valid-parentheses/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=WTzjTskDFMg)

  - `s = "()[]{}"` => `true`
  - store last opened in stack
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def isValid(self, s: str) -> bool:
        stack = []
        close_to_open = {"}": "{", ")": "(", "]": "["}
        for p in s:
            if p in close_to_open:
                if not stack or close_to_open[p] != stack[-1]:
                    return False
                stack.pop()
            else:
                stack.append(p)
        return len(stack) == 0
    ```

    </details>

- 🗿[**Min Stack**](https://leetcode.com/problems/min-stack/?md)⛅:
  [💡](https://www.youtube.com/watch?v=qkLl7nAwDPo)

  - `push(val)`, `pop()`, `top()`, `getMin()`
  - stack, append((min_so_far, val))
  - <details>
      <summary>O(1) time, O(n) space</summary>

    ```python
    def __init__(self):
        self.stack = []

    def push(self, val: int) -> None:
        if not self.stack:
            self.stack.append((val, val))
        else:
            min_so_far = min(val, self.stack[-1][0])
            self.stack.append((min_so_far, val))

    def pop(self) -> None:
        return self.stack.pop()[1]


    def top(self) -> int:
        return self.stack[-1][1]


    def getMin(self) -> int:
        return self.stack[-1][0]
    ```

    </details>

- 🗿 🏢[**Reverse Polish Notation**](https://leetcode.com/problems/evaluate-reverse-polish-notation/?md)⛅:
  [💡](https://youtu.be/iu0082c4HDE)

  - `tokens = ["2","1","+","3","*"]` => `9`
  - `b, a = stack.pop(), stack.pop()` ... `return stack[0]`
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        operators = {
            "+": lambda a,b: a+b,
            "-": lambda a,b: a-b,
            "*": lambda a,b: a*b,
            "/": lambda a,b: int(a/b),
        }
        for t in tokens:
            if t not in operators:
                stack.append(int(t))
            else:
                b, a = stack.pop(), stack.pop()
                stack.append(operators[t](a, b))
        return stack[0]
    ```

    </details>

- 🗿[**Generate Parentheses**](https://leetcode.com/problems/generate-parentheses/?md)⛅:
  [💡](https://www.youtube.com/watch?v=s9fokUqJ76A)

  - `n = 3` => `["((()))","(()())","(())()","()(())","()()()"]`
  - stack, backtracking(opened, closed)
  - <details>
      <summary>Exponential</summary>

    ```python
    def generateParenthesis(self, n: int) -> List[str]:
        result = []
        path = []

        def dfs(opened, closed):
            if opened == closed == n:
                result.append("".join(path))
                return
            if opened < n:
                path.append("(")
                dfs(opened+1, closed)
                path.pop()

            if closed < opened:
                path.append(")")
                dfs(opened, closed+1)
                path.pop()

        dfs(0, 0)
        return result
    ```

    </details>

- 🗿[**Daily Temperatures**](https://leetcode.com/problems/daily-temperatures/?md)⛅:
  [💡](https://www.youtube.com/watch?v=cTBiBSnjO3c)

  - `T = [73,74,75,71,69,72,76,73]` => `[1,1,4,2,1,1,0,0]`
  - stack of pending indices, while current > stack[-1], pop and update ans
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        res = [0] * len(temperatures)
        stack = []

        for i, t in enumerate(temperatures):
            while stack and t > stack[-1][0]:
                stackT, stackInd = stack.pop()
                res[stackInd] = i - stackInd
            stack.append((t, i))
        return res
    ```

    </details>

- 🗿[**Car fleet**](https://leetcode.com/problems/car-fleet/?md)⛅:
  [💡](https://www.youtube.com/watch?v=Pr6T-3yB9RM)

  - `target = 12, position = [10,8,0,5,3], speed = [2,4,1,1,3]` => `3`
  - Sort by position, start from end, if time to reach is faster than prev, ignore
  - <details>
      <summary>O(nlogn) time, O(n) space</summary>

    ```python
    def carFleet(self, target: int, position: List[int], speed: List[int]) -> int:
        stack = []
        for p, s in sorted([(p,s) for p, s in zip(position, speed)], reverse=True):
            time = (target-p)/s
            if stack and time <= stack[-1]:
                continue
            stack.append(time)
        return len(stack)
    ```

    </details>

- 🗿[**Largest Rectangle in Histogram**](https://leetcode.com/problems/largest-rectangle-in-histogram/?md)⛈️:
  [💡](https://www.youtube.com/watch?v=zx5Sw9130L0)

  - `heights = [2,1,5,6,2,3]` => `10`
  - stack, pop bigger than current, calculate area
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def largestRectangleArea(self, heights: List[int]) -> int:
        maxArea = 0
        stack = []

        for i, h in enumerate(heights):
            start = i
            while stack and stack[-1][1] > h:
                index, height = stack.pop()
                maxArea = max(maxArea, height * (i - index))
                start = index
            stack.append((start, h))

        for i, h in stack:
            maxArea = max(maxArea, h * (len(heights) - i))
        return maxArea
    ```

    </details>

## Binary Search

Remember it can be used on a range.

---

- 🗿[**Binary Search**](https://leetcode.com/problems/binary-search/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=s4DPM8ct1pI)

  - `nums = [-1,0,3,5,9,12], target = 9` => `4`
  - while l <= r, mid = (l+r)//2
  - <details>
      <summary>O(logn) time, O(1) space</summary>

    ```python
    def search(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums)-1
        while l <= r:
            mid = (l+r)//2
            if nums[mid] == target:
                return mid
            if nums[mid] < target:
                l = mid + 1
            else:
                r = mid - 1
        return -1
    ```

    </details>

- 🗿[**Search a 2D Matrix**](https://leetcode.com/problems/search-a-2d-matrix/?md)⛅:
  [💡](https://www.youtube.com/watch?v=Ber2pi2C0j0)

  - `matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3` => `true`
  - `row = mid // len(matrix[0])`, `col = mid % len(matrix[0])`
  - <details>
      <summary>O(logmn) time, O(1) space</summary>

    ```python
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        l, r = 0, len(matrix)*len(matrix[0])-1
        while l <= r:
            mid = (l+r)//2
            row = mid // len(matrix[0])
            col = mid % len(matrix[0])
            if matrix[row][col] == target:
                return True
            if matrix[row][col] < target:
                l = mid+1
            else:
                r = mid-1
        return False
    ```

    </details>

- 🗿[**Koko Eating Bananas**](https://leetcode.com/problems/koko-eating-bananas/?md)⛅:
  [💡](https://www.youtube.com/watch?v=U2SozAs9RzA)

  - `piles = [3,6,7,11], H = 8` => `4`
  - `l, r = 1, max(piles)`, `sum(math.ceil(p/mid) for p in piles) <= h`
  - <details>
      <summary>O(nlogm) time, O(1) space</summary>

    ```python
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        l, r = 1, max(piles)
        result = r
        while l <= r:
            mid = (l+r)//2
            hours = sum(math.ceil(p/mid) for p in piles)
            if hours <= h:
                result = min(result, mid)
                r = mid-1
            else:
                l = mid+1
        return result
    ```

    </details>

- 🅱️[**Search in Rotated Sorted Array**](https://leetcode.com/problems/search-in-rotated-sorted-array/?md)⛅:
  [💡](https://www.youtube.com/watch?v=U8XENwh8Oy8)

  - `nums = [4,5,6,7,0,1,2], target = 0` => `4`
  - see which side is sorted and then `if target > nums[mid] or target < nums[l]`
  - <details>
      <summary>O(logn) time, O(1) space</summary>

    ```python
    def search(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums)-1

        while l <= r:
            mid = (l+r) // 2
            if target == nums[mid]:
                return mid

            # left sorted
            if nums[l] <= nums[mid]:
                if nums[l] <= target <= nums[mid]:
                    r = mid-1
                else:
                    l = mid+1
            # right sorted
            else:
                if nums[mid] <= target <= nums[r]:
                    l = mid+1
                else:
                    r = mid-1
        return -1
    ```

    </details>

- 🅱️[**Find Minimum in Rotated Sorted Array**](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/?md)⛅:
  [💡](https://www.youtube.com/watch?v=nIVW4P8b1VA)

  - `nums = [3,4,5,1,2]` => `1`
  - stop `if nums[l] < nums[r]`, move `l` if `nums[mid] >= nums[l]`
  - <details>
      <summary>O(logn) time, O(1) space</summary>

    ```python
    def findMin(self, nums: List[int]) -> int:
        start , end = 0 ,len(nums) - 1
        curr_min = float("inf")

        while start  <  end :
            mid = (start + end ) // 2
            curr_min = min(curr_min,nums[mid])

            # right has the min
            if nums[mid] > nums[end]:
                start = mid + 1

            # left has the  min
            else:
                end = mid - 1

        return min(curr_min,nums[start])
    ```

    </details>

- 🗿[**Time Based Key-Value Store**](https://leetcode.com/problems/time-based-key-value-store/?md)⛅:
  [💡](https://www.youtube.com/watch?v=fu2cD_6E8Hw)

  - `set(k, v, time)`, `get(k, time)`
  - `if values[m][1] <= timestamp: res = values[m][0], l = m + 1`
  - <details>
      <summary>O(logn) time, O(n) space</summary>

    ```python
    def __init__(self):
        self.store = {}

    def set(self, key: str, value: str, timestamp: int) -> None:
        if key not in self.store:
            self.store[key] = []
        self.store[key].append((value, timestamp))

    def get(self, key: str, timestamp: int) -> str:
        if key not in self.store:
            return ""
        res = ""
        values = self.store[key]
        l, r = 0, len(values)-1
        while l <= r:
            m = (l+r)//2
            if values[m][1] <= timestamp:
                res = values[m][0]
                l = m+1
            else:
                r = m-1
        return res
    ```

    </details>

- 🗿 🏢[**Median of Two Sorted Arrays**](https://leetcode.com/problems/median-of-two-sorted-arrays/?md)⛈️:
  [💡](https://youtu.be/q6IEA26hvXc)

  - `nums1 = [1,2], nums2 = [3,4]` => `2.5`
  - Binary search on the shorter array until `Aleft <= Bright and Bleft <= Aright`.
  - <details>
      <summary>O(log(m+n)) time, O(1) space</summary>

    ```python
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        A, B = nums1, nums2
        total = len(nums1) + len(nums2)
        half = total // 2

        if len(B) < len(A):
            A, B = B, A

        l, r = 0, len(A) - 1
        while True:
            i = (l + r) // 2  # A
            j = half - i - 2  # B

            Aleft = A[i] if i >= 0 else float("-infinity")
            Aright = A[i + 1] if (i + 1) < len(A) else float("infinity")
            Bleft = B[j] if j >= 0 else float("-infinity")
            Bright = B[j + 1] if (j + 1) < len(B) else float("infinity")

            # partition is correct
            if Aleft <= Bright and Bleft <= Aright:
                # odd
                if total % 2:
                    return min(Aright, Bright)
                # even
                return (max(Aleft, Bleft) + min(Aright, Bright)) / 2
            elif Aleft > Bright:
                r = i - 1
            else:
                l = i + 1
    ```

    </details>

## Linked List

---

- 🅱️[**Reverse Linked List**](https://leetcode.com/problems/reverse-linked-list/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=G0_I-ZF0S38)

  - `head = [1,2]` => `[2,1]`
  - while cur: ..., tmp, update prev and cur, return prev
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prev = None
        cur = head

        while cur:
            next_cur = cur.next
            cur.next = prev
            prev = cur
            cur = next_cur
        return prev
    ```

    </details>

- 🅱️[**Merge Two Sorted Lists**](https://leetcode.com/problems/merge-two-sorted-lists/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=XIdigk956u0)

  - `l1 = [1,2,4], l2 = [1,3,4]` => `[1,1,2,3,4,4]`
  - dummy first node, curr node, while l1 and l2, add remaining
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        p1 = list1
        p2 = list2
        dummy = ListNode()
        cur = dummy

        while p1 and p2:
            if p1.val > p2.val:
                p1, p2 = p2, p1
            cur.next = p1
            cur = cur.next
            p1 = p1.next

        if p1: cur.next = p1
        if p2: cur.next = p2

        return dummy.next
    ```

    </details>

- 🅱️[**Merge K Sorted Lists**](https://leetcode.com/problems/merge-k-sorted-lists/?md)⛈️:
  [💡](https://www.youtube.com/watch?v=q5a5OiGbT6Q)

  - `lists = [[1,4,5],[1,3,4],[2,6]]` => `[1,1,2,3,4,4,5,6]`
  - Merge two lists at a time.
  - <details>
      <summary>O(nlogk) time, O(1) space</summary>

    ```python
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        if not lists or len(lists) == 0:
            return None

        while len(lists) > 1:
            mergedLists = []
            for i in range(0, len(lists), 2):
                l1 = lists[i]
                l2 = lists[i + 1] if (i + 1) < len(lists) else None
                mergedLists.append(self.mergeList(l1, l2))
            lists = mergedLists
        return lists[0]
    ```

    </details>

- 🅱️[**Reorder List**](https://leetcode.com/problems/reorder-list/?md)⛅
  [💡](https://www.youtube.com/watch?v=S5bfdUTrKLM)

  - `head = [1,2,3,4]` => `[1,4,2,3]` (L0 -> Ln -> L1 -> Ln-1 -> L2...)
  - find mid, reverse second half, merge two lists
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def reorderList(self, head: Optional[ListNode]) -> None:
        slow = head
        fast = head.next
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

        p1 = head
        p2 = self.reverse(slow)
        # merge
        while p1 and p2:
            tmp1 = p1.next
            tmp2 = p2.next
            p1.next = p2
            p2.next = tmp1
            p1 = tmp1
            p2 = tmp2
        return head

    def reverse(self, node):
        ...
    ```

    </details>

- 🅱️[**Remove Nth Node From End of List**](https://leetcode.com/problems/remove-nth-node-from-end-of-list/?md)⛅:
  [💡](https://www.youtube.com/watch?v=XVuQxVej6y8)

  - `head = [1,2,3,4,5], n = 2` => `[1,2,3,5]`
  - Two pointers, move one n steps ahead.
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        fast = slow = head
        for _ in range(n):
            fast = fast.next
        if not fast:
            return head.next
        while fast.next:
            fast = fast.next
            slow = slow.next
        slow.next = slow.next.next
        return head
    ```

    </details>

- 🗿[**Copy List with Random Pointer**](https://leetcode.com/problems/copy-list-with-random-pointer/?md)⛅:
  [💡](https://www.youtube.com/watch?v=5Y2EiZST97Y)

  - `head = [[3,null],[3,0],[3,null]]` => `[[3,null],[3,0],[3,null]]`
  - two passes, copy and then update next and random, `old_to_new = {None: None}`
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def copyRandomList(self, head: 'Optional[Node]') -> 'Optional[Node]':
        old_to_new = {None: None}
        cur = head
        while cur:
            copy = Node(cur.val)
            old_to_new[cur] = copy
            cur = cur.next

        cur = head
        while cur:
            copy = old_to_new[cur]
            copy.next = old_to_new[cur.next]
            copy.random = old_to_new[cur.random]
            cur = cur.next
        return old_to_new[head]
    ```

    </details>

- 🗿 🏢[**Add Two Numbers**](https://leetcode.com/problems/add-two-numbers/?md)⛅:
  [💡](https://www.youtube.com/watch?v=wgFPrzTjm7s)

  - `l1 = [2,4,3], l2 = [5,6,4]` => `[7,0,8]`
  - carry = sum\_ // 10, remain = p1 or p2, etc.
  - <details>
      <summary>O(max(m,n)) time, O(max(m,n)) space</summary>

    ```python
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        p1 = l1
        p2 = l2
        dummy = ListNode()
        cur = dummy
        carry = 0

        while p1 and p2:
           sum_ = p1.val + p2.val + carry
           cur.next = ListNode(sum_%10)
           cur = cur.next
           p1 = p1.next
           p2 = p2.next
           carry = sum_ // 10

        remainder = p1 or p2
        while remainder:
            sum_ = remainder.val + carry
            cur.next = ListNode(sum_%10)
            cur = cur.next
            remainder = remainder.next
            carry = sum_ // 10

        if carry:
            cur.next = ListNode(1)

        return dummy.next
    ```

    </details>

- 🅱️[**Linked List Cycle**](https://leetcode.com/problems/linked-list-cycle/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=gBTe7lFR3vc)

  - `head = [3,2,0,-4], -4 -> 2` => `true`
  - slow = fast = head, while fast and fast.next:
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        slow = head
        fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True
        return False
    ```

    </details>

- 🗿[**Find The Duplicate Number**](https://leetcode.com/problems/find-the-duplicate-number/?md)⛅:
  [💡](https://www.youtube.com/watch?v=wjYnzkAhcNk)

  - `nums = [1,3,4,2,2]` => `2`
  - Floyd's cycle detection, `slow = nums[slow]; fast = nums[nums[fast]]`, slow2
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python

    ```

    </details>

- 🗿[**LRU Cache**](https://leetcode.com/problems/lru-cache/?md)⛅:
  [💡](https://www.youtube.com/watch?v=7ABFKPK2hD4)

  - `LRUCache(int capacity)`, `get(int key)`, `put(int key, int value)`
  - double linked list, cache of key to node, dummy head and tail
  - <details>
      <summary>O(1) time, O(capacity) space</summary>

    ```python

    ```

    </details>

- 🗿[**Reverse Nodes in k-Group**](https://leetcode.com/problems/reverse-nodes-in-k-group/?md)⛈️:
  [💡](https://www.youtube.com/watch?v=1UOPsfP85V4)

  - `head = [1,2,3,4,5], k = 2` => `[2,1,4,3,5]`
  - groupPrev, groupNext, reverse `while curr != groupNext`
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python

    ```

    </details>

## Trees

---

Careful with recursion limit (bound to the application stack)

- 🅱️[**Invert Binary Tree**](https://leetcode.com/problems/invert-binary-tree/?ez)☀️:
  [💡](https://leetcode.com/problems/invert-binary-tree/solutions/62705/python-solutions-recursively-dfs-bfs/?orderBy=most_votes)

  - `root = [4,2,7,1,3,6,9]` => `[4,7,2,9,6,3,1]`
  - `r.right, r.left = self.invert(r.left), self.invert(r.right)` or stack
  - <details>
      <summary>O(n) time, O(height) space</summary>

    ```python
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        stack = [root]
        while stack:
            node = stack.pop()
            if node is None:
                continue
            left = node.left
            right = node.right
            node.left = right
            node.right = left
            stack.append(left)
            stack.append(right)
        return root

    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None
        left, right = root.left, root.right
        root.left = self.invertTree(right)
        root.right = self.invertTree(left)
        return root
    ```

    </details>

- 🅱️[**Maximum Depth of Binary Tree**](https://leetcode.com/problems/maximum-depth-of-binary-tree/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=hTM3phVI6YQ)

  - `root = [3,9,20,null,null,15,7]` => `3`
  - `return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1`
  - <details>
      <summary>O(n) time, O(height) space</summary>

    ```python
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1
    ```

    </details>

- 🗿[**Diameter of Binary Tree**](https://leetcode.com/problems/diameter-of-binary-tree/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=bkxqA8Rfv04)

  - `root = [1,2,3,4,5]` => `3`
  - dfs, path: `res = max(res, left+right)`, max length: `return max(left, right) + 1`
  - <details>
      <summary>O(n) time, O(height) space</summary>

    ```python

    ```

    </details>

- 🗿[**Balanced Binary Tree**](https://leetcode.com/problems/balanced-binary-tree/?ez)☀️:
  [💡](https://leetcode.com/problems/balanced-binary-tree/solutions/407546/balanced-binary-tree/?ez=&orderBy=most_votes)

  - `root = [3,9,20,null,null,15,7]` => `true`
  - recursive, `abs(height(left)-height(right)) <= 1`
  - <details>
      <summary>O(n) time, O(height) space</summary>

    ```python

    ```

    </details>

- 🅱️[**Same Tree**](https://leetcode.com/problems/same-tree/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=vRbbcKXCxOw)

  - `p = [1,2,3], q = [1,2,3]` => `true`
  - `isSameTree(p.left, q.left) and isSameTree(p.right, q.right)`
  - <details>
      <summary>O(n) time, O(height) space</summary>

    ```python
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        if p is None and q is None:
            return True
        if p is None or q is None:
            return False
        return p.val == q.val and self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
    ```

    </details>

- 🅱️[**Subtree of Another Tree**](https://leetcode.com/problems/subtree-of-another-tree/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=E36O5SWp-LE)

  - `s = [3,4,5,1,2], t = [4,1,2]` => `true`
  - `isSameTree(s, t) or isSubtree(s.left, t) or isSubtree(s.right, t)`
  - <details>
      <summary>O(nm) time, O(n+m) space</summary>

    ```python
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        if not root:
            return False
        if self.is_same_tree(root, subRoot):
            return True
        return self.isSubtree(root.left, subRoot) or self.isSubtree(root.right, subRoot)
    ```

    </details>

- 🅱️[**Binary Tree Level Order Traversal**](https://leetcode.com/problems/binary-tree-level-order-traversal/?md)⛅:
  [💡](https://leetcode.com/problems/binary-tree-level-order-traversal/solutions/33464/5-6-lines-fast-python-solution-48-ms/)

  - `root = [3,9,20,null,null,15,7]` => `[[3],[9,20],[15,7]]`
  - `while level: ans.append([node.val for node in level]); level = [...]` or queue
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        level = [root]
        result = []
        while level:
            new_level = []
            level = [l for l in level if l]
            if level:
                result.append([n.val for n in level])
            for n in level:
                new_level.append(n.left)
                new_level.append(n.right)
            level = new_level
        return result
    ```

    </details>

- 🗿[**Binary Tree Right Side View**](https://leetcode.com/problems/binary-tree-right-side-view/?md)⛅:
  [💡](https://www.youtube.com/watch?v=d4zLyf32e3I)

  - `root = [1,2,3,null,5,null,4]` => `[1,3,4]`
  - Level order, right most
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Count Good Nodes in Binary Tree**](https://leetcode.com/problems/count-good-nodes-in-binary-tree/?md)⛅:
  [💡](https://www.youtube.com/watch?v=7cp5imvDzl4)

  - `root = [3,1,4,3,null,1,5]` => `4`
  - dfs, stack = [(node, max_so_far)]
  - <details>
      <summary>O(n) time, O(height) space</summary>

    ```python

    ```

    </details>

- 🅱️[**Construct Binary Tree from Preorder and Inorder Traversal**](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/?md)⛅:
  [💡](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/solutions/34579/python-short-recursive-solution/)

  - `preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]` => `[3,9,20,null,null,15,7]`
  - ...
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def buildTree(self, preorder, inorder):
    if inorder:
        ind = inorder.index(preorder.pop(0))
        root = TreeNode(inorder[ind])
        root.left = self.buildTree(preorder, inorder[0:ind])
        root.right = self.buildTree(preorder, inorder[ind+1:])
        return root
    ```

    </details>

- 🅱️[**Binary Tree Maximum Path Sum**](https://leetcode.com/problems/binary-tree-maximum-path-sum/?md)⛈️:
  [💡](https://leetcode.com/problems/binary-tree-maximum-path-sum/solutions/603423/python-recursion-stack-thinking-process-diagram/)

  - `root = [1,2,3]` => `6`
  - dfs, allow split or not, nonlocal max
  - <details>
      <summary>O(n) time, O(height) space</summary>

    ```python
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        max_path = float("-inf")
        def get_max_gain(node):
            nonlocal max_path
            if node is None:
                return 0

            gain_on_left = max(get_max_gain(node.left), 0)
            gain_on_right = max(get_max_gain(node.right), 0)
            current_max_path = node.val + gain_on_left + gain_on_right
            max_path = max(max_path, current_max_path)

            return node.val + max(gain_on_left, gain_on_right)

        get_max_gain(root)
        return max_path
    ```

    </details>

- 🅱️[**Serialize and Deserialize Binary Tree**](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/?md)⛈️:
  [💡](https://www.youtube.com/watch?v=u4JAi2JJhI8)

  - `serialize(self, root)` and `deserialize(self, data)`
  - Preorder travelsal, `"1,2,N,N,3,4,N,N,5,N,N"`
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def serialize(self, root):
        values = []
        def preorder(node):
            if node:
                values.append(str(node.val))
                preorder(node.left)
                preorder(node.right)
            else:
                values.append('#')
        preorder(root)
        return ' '.join(values)

    def deserialize(self, data):
        values = list(reversed(data.split()))
        def preorder():
            value = values.pop()
            if value == '#':
                return None
            node = TreeNode(int(value))
            node.left = preorder()
            node.right = preorder()
            return node
        return preorder()
    ```

    </details>

> BST: nodes left < node < nodes right

- 🅱️[**Lowest Common Ancestor of a Binary Search Tree**](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/?md)⛅:
  [💡](https://www.youtube.com/watch?v=gs2LMfuOR9k)

  - `root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8` => `6`
  - cur, left `if p<cur and q<cur`, right `if p>cur and q>cur` else return
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        cur = root
        while cur:
            if p.val < cur.val and q.val < cur.val:
                cur = cur.left
            elif p.val > cur.val and q.val > cur.val:
                cur = cur.right
            else:
                return cur
    ```

    </details>

- 🅱️[**Validate Binary Search Tree**](https://leetcode.com/problems/validate-binary-search-tree/?md)⛅:
  [💡](https://www.youtube.com/watch?v=s6ATEkipzow)

  - `root = [2,1,3]` => `true`
  - `validate(n.left, min_val, n.val) and validate(n.right, n.val, max_val)`
  - <details>
      <summary>O(n) time, O(height) space</summary>

    ```python
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        return self.validate(root)

    def validate(self, node, min_val=None, max_val=None):
        if not node:
            return True
        if min_val is not None and node.val <= min_val:
            return False
        if max_val is not None and node.val >= max_val:
            return False
        return self.validate(node.left, min_val, node.val) and self.validate(node.right, node.val, max_val)
    ```

    </details>

- 🅱️[**Kth Smallest Element in a BST**](https://leetcode.com/problems/kth-smallest-element-in-a-bst/?md)⛅
  [💡](https://www.youtube.com/watch?v=5LUXSvjmGCw)

  - `root = [3,1,4,null,2], k = 1` => `1`
  - inorder traversal, return res[k-1] (or iterative inorder with stack)
  - <details>
      <summary>O(n) time, O(height) space</summary>

    ```python
     def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        count = []
        self.helper(root, count)
        return count[k-1]

    def helper(self, node, count):
        if not node:
            return

        self.helper(node.left, count)
        count.append(node.val)
        self.helper(node.right, count)
    ```

    </details>

## Tries

---

- 🅱️[**Implement Trie (Prefix Tree)**](https://leetcode.com/problems/implement-trie-prefix-tree/?md)⛅:
  [💡](https://www.youtube.com/watch?v=oobqoCJlHA0)

  - `insert(word)`, `search(word)`, `startsWith(prefix)`
  - node with dict[char, node] and a bool isWord.
  - O(n) time, O(n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    class TrieNode:
        def __init__(self):
            self.children = [None] * 26
            self.end = False


    class Trie:
        def __init__(self):
            """
            Initialize your data structure here.
            """
            self.root = TrieNode()

        def insert(self, word: str) -> None:
            """
            Inserts a word into the trie.
            """
            curr = self.root
            for c in word:
                i = ord(c) - ord("a")
                if curr.children[i] == None:
                    curr.children[i] = TrieNode()
                curr = curr.children[i]
            curr.end = True

        def search(self, word: str) -> bool:
            """
            Returns if the word is in the trie.
            """
            curr = self.root
            for c in word:
                i = ord(c) - ord("a")
                if curr.children[i] == None:
                    return False
                curr = curr.children[i]
            return curr.end

        def startsWith(self, prefix: str) -> bool:
            """
            Returns if there is any word in the trie that starts with the given prefix.
            """
            curr = self.root
            for c in prefix:
                i = ord(c) - ord("a")
                if curr.children[i] == None:
                    return False
                curr = curr.children[i]
            return True
    ```

    </details>

- 🅱️[**Design Add And Search Words Data Structure**](https://leetcode.com/problems/design-add-and-search-words-data-structure/?md)⛅:
  [💡](https://www.youtube.com/watch?v=BTf05gs_8iU)

  - `addWord(word)`, `search(word)`
  - trie, recursive dfs(idx, node) for "."
  - O(n) time, O(n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    
    class TrieNode:
        def __init__(self):
            self.children = {}  # a : TrieNode
            self.word = False


    class WordDictionary:
        def __init__(self):
            self.root = TrieNode()

        def addWord(self, word: str) -> None:
            cur = self.root
            for c in word:
                if c not in cur.children:
                    cur.children[c] = TrieNode()
                cur = cur.children[c]
            cur.word = True

        def search(self, word: str) -> bool:
            def dfs(j, root):
                cur = root

                for i in range(j, len(word)):
                    c = word[i]
                    if c == ".":
                        for child in cur.children.values():
                            if dfs(i + 1, child):
                                return True
                        return False
                    else:
                        if c not in cur.children:
                            return False
                        cur = cur.children[c]
                return cur.word

            return dfs(0, self.root)
    ```

    </details>

- 🅱️[**Word Search II**](https://leetcode.com/problems/word-search-ii/?md)⛈️
  [💡](https://www.youtube.com/watch?v=asbcE9mZz_U)

  - `board, words = ["oath","pea","eat","rain"]` => `["eat","oath"]`
  - build words trie from each cell, dfs
  - O(mn\*4^mn) time, O(n) space (brute force O(wmn\*4^mn))
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    class TrieNode:
        def __init__(self):
            self.children = {}
            self.isWord = False
            self.refs = 0

        def addWord(self, word):
            cur = self
            cur.refs += 1
            for c in word:
                if c not in cur.children:
                    cur.children[c] = TrieNode()
                cur = cur.children[c]
                cur.refs += 1
            cur.isWord = True

        def removeWord(self, word):
            cur = self
            cur.refs -= 1
            for c in word:
                if c in cur.children:
                    cur = cur.children[c]
                    cur.refs -= 1


    class Solution:
        def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
            root = TrieNode()
            for w in words:
                root.addWord(w)

            ROWS, COLS = len(board), len(board[0])
            res, visit = set(), set()

            def dfs(r, c, node, word):
                if (
                    r not in range(ROWS) 
                    or c not in range(COLS)
                    or board[r][c] not in node.children
                    or node.children[board[r][c]].refs < 1
                    or (r, c) in visit
                ):
                    return

                visit.add((r, c))
                node = node.children[board[r][c]]
                word += board[r][c]
                if node.isWord:
                    node.isWord = False
                    res.add(word)
                    root.removeWord(word)

                dfs(r + 1, c, node, word)
                dfs(r - 1, c, node, word)
                dfs(r, c + 1, node, word)
                dfs(r, c - 1, node, word)
                visit.remove((r, c))

            for r in range(ROWS):
                for c in range(COLS):
                    dfs(r, c, root, "")

            return list(res)
    ```

    </details>

## Heap & Priority Queue

---

> - Binary Tree
> - Heap invariant: each node is <= than its children.
> - Implemented as array: root at `0`, children `2i+1` & `2i+2`, parent at `(i-1)//2`

- 🗿[**Kth Largest Element in a Stream**](https://leetcode.com/problems/kth-largest-element-in-a-stream/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=hOjcdrqMoQ8)

  - `KthLargest(int k, int[] nums)`, `add(num) -> int`
  - simple minheap, pop if len > k, return heap[0]
  - O(logk) time, O(k) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Last Stone Weight**](https://leetcode.com/problems/last-stone-weight/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=B-QCq79-Vfw)

  - `stones = [2,7,4,1,8,1]` => `1`
  - Heapify, pop 2 largest, push diff, repeat while len > 1
  - O(nlogn) time, O(n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**K Closests Points to Origin**](https://leetcode.com/problems/k-closest-points-to-origin/?md)⛅:
  [💡](https://www.youtube.com/watch?v=rI2EBUEMfTk)

  - `points = [[1,3],[-2,2]], K = 1` => `[[-2,2]]`
  - simple maxheap (-dist, x, y), pop if len > k
  - O(nlogk) time, O(k) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Kth Largest Element in an Array**](https://leetcode.com/problems/kth-largest-element-in-an-array/?md)⛅:
  [💡](https://www.youtube.com/watch?v=XEmy13g1Qxc)

  - `nums = [3,2,1,5,6,4], k = 2` => `5`
  - quickselect,
  - O(2n) average, O(n^2) worst case time, O(1) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Task Scheduler**](https://leetcode.com/problems/task-scheduler/?md)⛅:
  [💡](https://www.youtube.com/watch?v=s8p8ukTyA2I)

  - `tasks = ["A","A","A","B","B","B"], n = 2` => `8`
  - max_heap of times, queue, `while max_heap or q`, increase time
  - O(n) time, O(1) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Design Twitter**](https://leetcode.com/problems/design-twitter/?md)⛅:
  [💡](https://www.youtube.com/watch?v=pNichitDD2E)

  - `postTweet`, `getNewsFeed`, `follow`, `unfollow`
  - heap([count, tweetid, foloweeid, index-1])
  - O(nlogn) time, O(n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🅱️[**Find Median from Data Stream**](https://leetcode.com/problems/find-median-from-data-stream/?hd)⛈️
  [💡](https://leetcode.com/problems/find-median-from-data-stream/solutions/74047/java-python-two-heap-solution-o-log-n-add-o-1-find/)

  - `addNum(num)` and `findMedian()`
  - 2 heaps, max heap for left, min heap for right, balance
  - O(logn) time, O(n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    class MedianFinder:
        def __init__(self):
            """
            initialize your data structure here.
            """
            # two heaps, large, small, minheap, maxheap
            # heaps should be equal size
            self.small, self.large = [], []  # maxHeap, minHeap (python default)

        def addNum(self, num: int) -> None:
            if self.large and num > self.large[0]:
                heapq.heappush(self.large, num)
            else:
                heapq.heappush(self.small, -1 * num)

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

    </details>

## Backtracking

- [Subsets](https://neetcode.io/courses/advanced-algorithms/11):
  - append to path, dfs, pop, dfs
  - if don't want duplicates while loop to increase idx before second dfs
- [Combinations](https://neetcode.io/courses/advanced-algorithms/12):
  ```python
  for j in range(i, n+1):
      path.append(j)
      dfs(j+1)
      path.pop()
  ```
- [Permutations](https://neetcode.io/courses/advanced-algorithms/13):
  ```python
  def backtrack(first = 0):
      if first == n:
          output.append(nums[:])
      for i in range(first, n):
          nums[first], nums[i] = nums[i], nums[first]
          backtrack(first + 1)
          nums[first], nums[i] = nums[i], nums[first]
  ```

---

- 🗿[**Subsets**](https://leetcode.com/problems/subsets/?md)⛅:
  [💡](https://www.youtube.com/watch?v=REOH22Xwdkk)

  - `nums = [1,2,3]` => `[[3],[1],[2],[1,2,3],[1,3],[2,3],[1,2],[]]`
  - dfs with backtracking, result and path
  - O(n \* 2^n) time, O(n) space (recursion stack)
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🅱️[**Combination Sum**](https://leetcode.com/problems/combination-sum/?md)⛅
  [💡](https://www.youtube.com/watch?v=GBKI9VSKdGg)

  - `candidates = [2,3,6,7], target = 7` => `[[7],[2,2,3]]`
  - dfs, backtracking, append path to global res
  - O(2^target) time, O(target) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []

        def dfs(i, cur, total):
            if total == target:
                res.append(cur.copy())
                return
            if i >= len(candidates) or total > target:
                return

            cur.append(candidates[i])
            dfs(i, cur, total + candidates[i])
            cur.pop()
            dfs(i + 1, cur, total)

        dfs(0, [], 0)
        return res
    ```

    </details>

- 🗿[**Permutations**](https://leetcode.com/problems/permutations/?md)⛅:
  [💡](https://www.youtube.com/watch?v=s7AvT7cGdSo)

  - `nums = [1,2,3]` => `[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]`
  - divide and conquer, remove first, recursive, add back
  - O(n^2\*n!) time, O(n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Subsets II**](https://leetcode.com/problems/subsets-ii/?md)⛅:
  [💡](https://www.youtube.com/watch?v=Vn2v6ajA7U0)

  - `nums = [1,2,2]` => `[[2],[1],[1,2,2],[2,2],[1,2],[]]`
  - sort, `while i+1 < len(nums) and nums[i] == nums[i+1]:`
  - O(n \* 2^n) time, O(n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Combination Sum II**](https://leetcode.com/problems/combination-sum-ii/?md)⛅:
  [💡](https://www.youtube.com/watch?v=rSA3t6BDDwg)

  - `candidates = [10,1,2,7,6,1,5], target = 8` => `[[1,1,6],[1,2,5],[1,7],[2,6]]`
  - dfs(pos, target), path = [], top if target <= 0
  - O(2^n) time, O(n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🅱️[**Word Search**](https://leetcode.com/problems/word-search/?md)⛅
  [💡](https://www.youtube.com/watch?v=pfiQ_PS1g8E)

  - `board = [["A","B","C","E"],["S","F","C","S"],...], word = "ABCCED"` => `true`
  - dfs, backtracking, mark visited
  - O(mn\*4^l) time, O(l) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def exist(self, board: List[List[str]], word: str) -> bool:
        ROWS, COLS = len(board), len(board[0])
        path = set()

        def dfs(r, c, i):
            if i == len(word):
                return True
            if (
                min(r, c) < 0
                or r >= ROWS
                or c >= COLS
                or word[i] != board[r][c]
                or (r, c) in path
            ):
                return False
            path.add((r, c))
            res = (
                dfs(r + 1, c, i + 1)
                or dfs(r - 1, c, i + 1)
                or dfs(r, c + 1, i + 1)
                or dfs(r, c - 1, i + 1)
            )
            path.remove((r, c))
            return res

        # To prevent TLE,reverse the word if frequency of the first letter is more than the last letter's
        count = defaultdict(int, sum(map(Counter, board), Counter()))
        if count[word[0]] > count[word[-1]]:
            word = word[::-1]
            
        for r in range(ROWS):
            for c in range(COLS):
                if dfs(r, c, 0):
                    return True
        return False

    # O(n * m * 4^n)
    ```

    </details>

- 🗿[**Palindrome Partitioning**](https://leetcode.com/problems/palindrome-partitioning/?md)⛅:
  [💡](https://www.youtube.com/watch?v=3jvWodd7ht0)

  - `s = "aab"` => `[["a","a","b"],["aa","b"]]`
  - res = [], path = [], dfs(pos), for j in range(i, len(s))
  - O(n\*2^n) time, O(n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Letter Combinations of a Phone Number**](https://leetcode.com/problems/letter-combinations-of-a-phone-number/?md)⛅:
  [💡](https://www.youtube.com/watch?v=0snEunUacZY)

  - `digits = "23"` => `["ad","ae","af","bd","be","bf","cd","ce","cf"]`
  - digit_to_char map, dfs, backtracking
  - O(n \* 4^n) time, O(n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**N Queens**](https://leetcode.com/problems/n-queens/?md)⛈️:
  [💡](https://www.youtube.com/watch?v=Ph95IHmRp5M)

  - `n = 4` => `[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]`
  - dfs(row): for c in range(n), col, pos_diag, neg_diag sets
  - O(n!) time, O(n²) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

## Graphs

---

- [Dijkstra's Algorithm](https://neetcode.io/courses/advanced-algorithms/14): shortest path algo, greedy, O(ElogV) time

  ```python
  shortest = {}
  heap = [(0, src)]
  while heap:
      w, node = heapq.heappop(heap)
      if node in shortest:
          continue
      shortest[node] = w
      for d, w2 in adj[node]:
          if d not in shortest:
              heapq.heappush(heap, (w + w2, d))
  ```

- [Prim's Algorithm](https://neetcode.io/courses/advanced-algorithms/15): MST, O(ElogV) time, O(V) space

  ```python
  heap = []
  for n, w in adj[0]:
      heapq.heappush(heap, (w, 0, n))
  visited = set()
  while heap:
      w, s, d = heapq.heappop(heap)
      if d in visited:
          continue
      mst.append((s, d))
      visited.add(d)
      for n, w2 in adj[d]:
          if n not in visited:
              heapq.heappush(heap, (w2, d, n))
  ```

- Kruskal: Union Find
- [Topological Sort](https://neetcode.io/courses/advanced-algorithms/17): Alien dict, dfs to the end and reverse, O(V+E) time, O(V) space
  ```python
  result = []
  #cycle = set()
  visited = set()
  def dfs(node):
      if node in visited:
          return
      visited.add(node)
      for n in adj[node]:
          dfs(n)
      result.append(node)
      #cycle.remove(node)
  for i in range(...):
      dfs(i)
  return result[::-1]
  ```

### Basic

- 🅱️🏢[**Number of Islands**](https://leetcode.com/problems/number-of-islands/?md)⛅:
  [💡](https://www.youtube.com/watch?v=pV2kpPD66nE)

  - `grid = [["1", "0"], ["0", "1"]]` => `2`
  - skip visited (mark 0 or set), dfs (4 directions) on 1s.
  - O(cells) time, O(cells) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid or not grid[0]:
            return 0

        islands = 0
        visit = set()
        rows, cols = len(grid), len(grid[0])

        def dfs(r, c):
            if (
                r not in range(rows)
                or c not in range(cols)
                or grid[r][c] == "0"
                or (r, c) in visit
            ):
                return

            visit.add((r, c))
            directions = [[0, 1], [0, -1], [1, 0], [-1, 0]]
            for dr, dc in directions:
                dfs(r + dr, c + dc)

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == "1" and (r, c) not in visit:
                    islands += 1
                    dfs(r, c)
        return islands
    ```

    </details>

- 🅱️[**Clone Graph**](https://leetcode.com/problems/clone-graph/?md)⛅:
  [💡](https://www.youtube.com/watch?v=mQeF6bN8hMk)

  - Node with val, neighbors.
  - cache[old] = copy, for n in neighbors: copy.neighbors.append(dfs(n))).
  - O(v+e) time, O(v) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def cloneGraph(self, node: "Node") -> "Node":
        oldToNew = {}

        def dfs(node):
            if node in oldToNew:
                return oldToNew[node]

            copy = Node(node.val)
            oldToNew[node] = copy
            for nei in node.neighbors:
                copy.neighbors.append(dfs(nei))
            return copy

        return dfs(node) if node else None
    ```

    </details>

- 🗿 🏢[**Max Area of Island**](https://leetcode.com/problems/max-area-of-island/?md)⛅:
  [💡](https://www.youtube.com/watch?v=iJGr1OtmH0c)

  - `grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],...,[0,0,0,0,0,0,0,1,1,1,0,0,0]]` => `6`
  - return 1 + dfs(4 directions), max_area = max(max_area, dfs)
  - O(mn) time, O(mn) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🅱️🏢[**Pacific Atlantic Water Flow**](https://leetcode.com/problems/pacific-atlantic-water-flow/?md)⛅:
  [💡](https://www.youtube.com/watch?v=s-VkcjHqkGI)

  - grid of heights.
  - start from edge and dfs(x, y, ocean, last_height) to higher, return set intersection
  - O(mn) time, O(mn) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        ROWS, COLS = len(heights), len(heights[0])
        pac, atl = set(), set()

        def dfs(r, c, visit, prevHeight):
            if (
                (r, c) in visit
                or r < 0
                or c < 0
                or r == ROWS
                or c == COLS
                or heights[r][c] < prevHeight
            ):
                return
            visit.add((r, c))
            dfs(r + 1, c, visit, heights[r][c])
            dfs(r - 1, c, visit, heights[r][c])
            dfs(r, c + 1, visit, heights[r][c])
            dfs(r, c - 1, visit, heights[r][c])

        for c in range(COLS):
            dfs(0, c, pac, heights[0][c])
            dfs(ROWS - 1, c, atl, heights[ROWS - 1][c])

        for r in range(ROWS):
            dfs(r, 0, pac, heights[r][0])
            dfs(r, COLS - 1, atl, heights[r][COLS - 1])

        res = []
        for r in range(ROWS):
            for c in range(COLS):
                if (r, c) in pac and (r, c) in atl:
                    res.append([r, c])
        return res
    ```

    </details>

- 🗿[**Surrounded Regions**](https://leetcode.com/problems/surrounded-regions/?md)⛅:
  [💡](https://www.youtube.com/watch?v=9z2BunfoZ5Y)

  - `board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]`
  - dfs regions connected to edge, mark temporary, flip rest.
  - O(mn) time, O(1) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Rotting Oranges**](https://leetcode.com/problems/rotting-oranges/?md)⛅:
  [💡](https://www.youtube.com/watch?v=y704fEOx0s0)

  - `grid = [[2,1,1],[1,1,0],[0,1,1]]` => `4`
  - BFS, `while queue: minutes+=1 for i in range(len(queue))`, check if any remain
  - O(nm) time, O(nm) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Walls and Gates**](https://leetcode.com/problems/walls-and-gates/?md)⛅:
  [💡](https://www.youtube.com/watch?v=e69C6xhiSQE)

  - `rooms = [[2147483647,-1,0,2147483647],...,[2147483647,2147483647,2147483647,-1]]`
  - BFS, `while queue` pop left, add to queue if `rooms[r][c] > rooms[i][j]+1`
  - O(nm) time, O(nm) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🅱️[**Course Schedule**](https://leetcode.com/problems/course-schedule/?md)⛅:
  [💡](https://www.youtube.com/watch?v=EgI5nU9etnU)

  - DFS cycle detection (visited set, cycle set).
  - O(v+e) time, O(v) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        # dfs
        preMap = {i: [] for i in range(numCourses)}

        # map each course to : prereq list
        for crs, pre in prerequisites:
            preMap[crs].append(pre)

        visiting = set()

        def dfs(crs):
            if crs in visiting:
                return False
            if preMap[crs] == []:
                return True

            visiting.add(crs)
            for pre in preMap[crs]:
                if not dfs(pre):
                    return False
            visiting.remove(crs)
            preMap[crs] = []
            return True

        for c in range(numCourses):
            if not dfs(c):
                return False
        return True
    ```

    </details>

- 🗿 🏢[**Course Schedule II**](https://leetcode.com/problems/course-schedule-ii/?md)⛅:
  [💡](https://www.youtube.com/watch?v=Akt3glAwyfY)

  - `numCourses = 2, prerequisites = [[1,0]]` => `[0,1]`
  - DFS topological sort, return [] if cycle, visited and cycle set
  - O(v+e) time, O(v) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Redundant Connection**](https://leetcode.com/problems/redundant-connection/?md)⛅:
  [💡](https://www.youtube.com/watch?v=FXWRE67PLL0)

  - `edges = [[1,2], [1,3], [2,3]]` => `[2,3]`
  - union find, return edge if cycle (parent[x] == parent[y])
  - O(n+m) time, O(n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🅱️[**Number of Connected Components in an Undirected Graph**](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/?md)⛅:
  [💡](https://www.youtube.com/watch?v=8f1XPm4WOUc)

  - `n = 5, edges = [[0, 1], [1, 2], [3, 4]]` => `2`
  - union find
  - O(n+m) time, O(n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

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

    </details>

- 🅱️[**Graph Valid Tree**](https://leetcode.com/problems/graph-valid-tree/?md)⛅:
  [💡](https://www.youtube.com/watch?v=bXsUuownnoQ)

  - `n = 5, edges = [[0, 1], [0, 2], [0, 3], [1, 4]]` => `true`
  - adj list, DFS cycle detection, `dfs(0, prev=-1) and n == len(visit)`
  - O(e+v) time, O(e+v) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def validTree(self, n, edges):
        if not n:
            return True
        adj = {i: [] for i in range(n)}
        for n1, n2 in edges:
            adj[n1].append(n2)
            adj[n2].append(n1)

        visit = set()

        def dfs(i, prev):
            if i in visit:
                return False

            visit.add(i)
            for j in adj[i]:
                if j == prev:
                    continue
                if not dfs(j, i):
                    return False
            return True

        return dfs(0, -1) and n == len(visit)
    ```

    </details>

- 🗿[**Word Ladder**](https://leetcode.com/problems/word-ladder/?md)⛈️:
  [💡](https://www.youtube.com/watch?v=h9iTnkgv05E)

  - `beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]` => `5`
  - BFS, queue, create adj list with patterns `pattern = word[:i] + '*' + word[i+1:]`
  - O(m^2*n) time, O(m^2*n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

### Advanced

- 🗿[**Reconstruct Itinerary**](https://leetcode.com/problems/reconstruct-itinerary/?md)⛈️:
  [💡](https://www.youtube.com/watch?v=ZyB_gQ8vqGA)

  - `tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]` => `["JFK","MUC","LHR","SFO","SJC"]`
  - adj list, sort (lexico), dfs
  - O(n_flights^max_airport_f) time, O(n_airports+n_flights) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Min Cost to Connect All Points**](https://leetcode.com/problems/min-cost-to-connect-all-points/?md)⛅:
  [💡](https://www.youtube.com/watch?v=f7JOBJIC-NA)

  - `points = [[0,0],[2,2],[3,10],[5,2],[7,0]]` => `20`
  - Prim's algorithm, `weigth, s, d = heapq.heappop(heap)`
  - O(n^2) time, O(n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Network delay time**](https://leetcode.com/problems/network-delay-time/?md)⛅:
  [💡](https://www.youtube.com/watch?v=EaphyqKU4PQ)

  - `times = [[2,1,1],[2,3,1],[3,4,1]]`, `N = 4`, `K = 2` => `2`
  - Dijkstra's algorithm, visited set instead of dist
  - O(elogv) time, O(e+v) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿 🏢[**Swim in Rising Water**](https://leetcode.com/problems/swim-in-rising-water/?md)⛈️:
  [💡](https://www.youtube.com/watch?v=amvrKlMLuGY)

  - `grid = [[0,2],[1,3]]` => `3`
  - Simple Dijkstra, `heapq.heappush(heap, (max(t, grid[r][c]), r, c))`
  - O(n^2 log n) time, O(n^2) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🅱️[**Alien Dictionary**](https://leetcode.com/problems/alien-dictionary/?md)⛈️:
  [💡](https://www.youtube.com/watch?v=6kTZYvNNyps)

  - topological sort, DFS cycle detection.
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def alienOrder(self, words: List[str]) -> str:
        adj = {char: set() for word in words for char in word}

        for i in range(len(words) - 1):
            w1, w2 = words[i], words[i + 1]
            minLen = min(len(w1), len(w2))
            if len(w1) > len(w2) and w1[:minLen] == w2[:minLen]:
                return ""
            for j in range(minLen):
                if w1[j] != w2[j]:
                    print(w1[j], w2[j])
                    adj[w1[j]].add(w2[j])
                    break

        visited = {}  # {char: bool} False visited, True current path
        res = []

        def dfs(char):
            if char in visited:
                return visited[char]

            visited[char] = True

            for neighChar in adj[char]:
                if dfs(neighChar):
                    return True

            visited[char] = False
            res.append(char)

        for char in adj:
            if dfs(char):
                return ""

        res.reverse()
        return "".join(res)
    ```

    </details>

- 🗿[**Cheapest Flights Within K Stops**](https://leetcode.com/problems/cheapest-flights-within-k-stops/?md)⛅:
  [💡](https://www.youtube.com/watch?v=5eIK3zUdYmE)

  - `n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]`, `src = 0`, `dst = 2`, `k = 1` => `200`
  - BellmanFord, BFS (k+1 times), create `tmp_dist` and `distances = tmp_dist` at the end
  - O(k\*e) time, O(k\*e) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

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

- 🅱️[**Climbing Stairs**](https://leetcode.com/problems/climbing-stairs/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=Y0lT9Fck7qI) Fibonacci

  - `n = 2` => `2`, `n = 3` => `3`
  - `temp = n1 + n2`
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def climbStairs(self, n: int) -> int:
        if n == 1: return 1
        if n == 2: return 2
        cur = 0
        prev1, prev2 = 1, 2
        for i in range(3, n+1):
            cur = prev1 + prev2
            prev1 = prev2
            prev2 = cur
        return cur
    ```

    </details>

- 🗿[**Min cost climbing stairs**](https://leetcode.com/problems/min-cost-climbing-stairs/?ez)☀️:
  [💡](https://youtu.be/ktmzAZWkEZ0)

  - `cost = [10, 15, 20]` => `15` (start from 0 or 1, 1 or 2 steps)
  - `new = min(prev1+c[i-2], prev2+c[i-1]); prev1 = prev2; prev2 = new`
  - O(n) time, O(1) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🅱️[**House Robber**](https://leetcode.com/problems/house-robber/?md)⛅:
  [💡](https://leetcode.com/problems/house-robber/solutions/1605797/c-python-4-simple-solutions-w-explanation-optimization-from-brute-force-to-dp/)

  - `nums = [2,7,9,3,1]` => `12` (2, 9, 1)
  - `rob1, rob2 = 0, 0`, `tmp = max(rob1 + n, rob2)`, `return rob2`
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def rob(self, nums: List[int]) -> int:
        rob1, rob2 = 0, 0
        for n in nums:
            temp = max(n+rob1, rob2)
            rob1 = rob2
            rob2 = temp
        return rob2
    ```

    </details>

- 🅱️[**House Robber II**](https://leetcode.com/problems/house-robber-ii/?md)⛅:
  [💡](https://www.youtube.com/watch?v=rWAJCfYYOvM)

  - `nums = [2,3,2]` => `3` (circular)
  - `max(nums[0], rob1(nums[1:]), rob1(nums[:-1]))`
  - O(n) time, O(1) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def rob(self, nums: List[int]) -> int:
        return max(nums[0], self.helper(nums[1:]), self.helper(nums[:-1]))

    def helper(self, nums):
        rob1, rob2 = 0, 0

        for n in nums:
            newRob = max(rob1 + n, rob2)
            rob1 = rob2
            rob2 = newRob
        return rob2
    ```

    </details>

- 🅱️🏢[**Longest Palindromic Substring**](https://leetcode.com/problems/longest-palindromic-substring/?md)⛅:
  [💡](https://www.youtube.com/watch?v=XYQecbcd6_c)

  - `s = "babad"` => `"bab"` or `"aba"`
  - for i in range(len(s)): expand around l, r = 1, 1 and l, r = i, i+1
  - O(n^2) time, O(1) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def longestPalindrome(self, s: str) -> str:
        res = ""
        resLen = 0

        for i in range(len(s)):
            # odd length
            l, r = i, i
            while l >= 0 and r < len(s) and s[l] == s[r]:
                if (r - l + 1) > resLen:
                    res = s[l : r + 1]
                    resLen = r - l + 1
                l -= 1
                r += 1

            # even length
            l, r = i, i + 1
            while l >= 0 and r < len(s) and s[l] == s[r]:
                if (r - l + 1) > resLen:
                    res = s[l : r + 1]
                    resLen = r - l + 1
                l -= 1
                r += 1

        return res
    ```

    </details>

- 🅱️[**Palindrome Substrings**](https://leetcode.com/problems/palindromic-substrings/?md)⛅:
  [💡](https://www.youtube.com/watch?v=4RACzI5-du8)

  - `s = "abc"` => `3` (a, b, c)
  - similar to the preview one, expand and add to count
  - O(n^2) time, O(1) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def countSubstrings(self, s: str) -> int:
        res = 0

        for i in range(len(s)):
            res += self.countPali(s, i, i)
            res += self.countPali(s, i, i + 1)
        return res

    def countPali(self, s, l, r):
        res = 0
        while l >= 0 and r < len(s) and s[l] == s[r]:
            res += 1
            l -= 1
            r += 1
        return res
    ```

    </details>

- 🅱️[**Decode Ways**](https://leetcode.com/problems/decode-ways/?md)⛅:
  [💡](https://www.youtube.com/watch?v=6aEyTjOwlJU)

  - `s = "12"` => `2` ("AB" or "L")
  - start from end, `dp[i] = dp[i+1]` add `dp[i+2]` if `dp[i:i+2]` is valid
  - O(n) time, O(n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def numDecodings(self, s: str) -> int:
        # Memoization
        dp = {len(s): 1}

        def dfs(i):
            if i in dp:
                return dp[i]
            if s[i] == "0":
                return 0

            res = dfs(i + 1)
            if i + 1 < len(s) and (
                s[i] == "1" or s[i] == "2" and s[i + 1] in "0123456"
            ):
                res += dfs(i + 2)
            dp[i] = res
            return res

        return dfs(0)

        # Dynamic Programming
        dp = {len(s): 1}
        for i in range(len(s) - 1, -1, -1):
            if s[i] == "0":
                dp[i] = 0
            else:
                dp[i] = dp[i + 1]

            if i + 1 < len(s) and (
                s[i] == "1" or s[i] == "2" and s[i + 1] in "0123456"
            ):
                dp[i] += dp[i + 2]
        return dp[0]
    ```

    </details>

- 🅱️[**Coin Change**](https://leetcode.com/problems/coin-change/?md)⛅:
  [💡](https://www.youtube.com/watch?v=H9bfqozjoqs) Unbounded Knapsack

  - `coins = [1,2,5], amount = 11` => `3`
  - Cache and iterate `range(amount+1)`, recursive is O(coins^amount)
  - O(coins\*amount) time, O(amount) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [amount + 1] * (amount + 1)
        dp[0] = 0

        for a in range(1, amount + 1):
            for c in coins:
                if a - c >= 0:
                    dp[a] = min(dp[a], 1 + dp[a - c])
        return dp[amount] if dp[amount] != amount + 1 else -1
    ```

    </details>

- 🅱️[**Maximum Product Subarray**](https://leetcode.com/problems/maximum-product-subarray/?md)⛅:
  [💡](https://www.youtube.com/watch?v=lXVy6YWFcRM)

  - `nums = [2,3,-2,4]` => `6` (2, 3)
  - `cur_max, cur_min, max_prod = 1, 1, float('-inf')`, reset if n == 0
  - O(n) time, O(1) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def maxProduct(self, nums: List[int]) -> int:
        # O(n)/O(1) : Time/Memory
        res = nums[0]
        curMin, curMax = 1, 1

        for n in nums:

            tmp = curMax * n
            curMax = max(n * curMax, n * curMin, n)
            curMin = min(tmp, n * curMin, n)
            res = max(res, curMax)
        return res
    ```

    </details>

- 🅱️[**Word Break**](https://leetcode.com/problems/word-break/?md)⛅:
  [💡](https://www.youtube.com/watch?v=Sx9NNgInc3A)
  - `s = "leetcode", words = ["leet", "code"]` => `true`
  - start from end, `dp[i] = any(dp[i+len(w)] for w in words if s[i:i+len(w)] == w)`
  - <details>
      <summary>O(n^2) time, O(n) space</summary>

    ```python
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:

        dp = [False] * (len(s) + 1)
        dp[len(s)] = True

        for i in range(len(s) - 1, -1, -1):
            for w in wordDict:
                if (i + len(w)) <= len(s) and s[i : i + len(w)] == w:
                    dp[i] = dp[i + len(w)]
                if dp[i]:
                    break

        return dp[0]
    ```

    </details>
- 🅱️[**Longest Increasing Subsequence**](https://leetcode.com/problems/longest-increasing-subsequence/?md)⛅:
  [💡](https://www.youtube.com/watch?v=cjWnW0hdF1Y)

  - `nums = [10,9,2,5,3,7,101,18]` => `4` (2, 3, 7, 101) strict
  - start from end, `if nums[i] < nums[j]: dp[i] = max(dp[i], dp[j] + 1)`
  - O(n^2) time, O(n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def lengthOfLIS(self, nums: List[int]) -> int:
        LIS = [1] * len(nums)

        for i in range(len(nums) - 1, -1, -1):
            for j in range(i + 1, len(nums)):
                if nums[i] < nums[j]:
                    LIS[i] = max(LIS[i], 1 + LIS[j])
        return max(LIS)
    ```

    </details>

- 🗿[**Partition Equal Subset Sum**](https://leetcode.com/problems/partition-equal-subset-sum/?md)⛅:
  [💡](https://www.youtube.com/watch?v=IsvocB5BJhw)

  - `nums = [1,5,11,5]` => `true` (1, 5, 5) and (11)
  - target is sum//2, `possible = possible.union({p+n for p in possible})`
  - O(n\*target) time, O(target) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

### 2D

- 🅱️🏢[**Unique Paths**](https://leetcode.com/problems/unique-paths/?md)⛅
  [💡](https://www.youtube.com/watch?v=IlEsdxuD4lY)

  - `m = 3, n = 2` => `3`
  - `cache[0, 0] = 1; cache[i, j] = cache.get((i-1, j), 0) + cache.get((i, j-1), 0)`
  - <details>
      <summary>O(m*n) time, O(n) space</summary>

    ```python
    def uniquePaths(self, m: int, n: int) -> int:
        cache = {
            (i, j): 0
            for i in range(m)
            for j in range(n)
        }
        cache[0, 0] = 1
        for i in range(m):
            for j in range(n):
                if (i, j) == (0, 0):
                    continue
                cache[i, j] = cache.get((i-1, j), 0) + cache.get((i, j-1), 0)

        return cache[m-1, n-1]

    def uniquePaths(self, m: int, n: int) -> int:
      """O(n) space"""
        row = [1] * n

        for i in range(m - 1):
            newRow = [1] * n
            for j in range(n - 2, -1, -1):
                newRow[j] = newRow[j + 1] + row[j]
            row = newRow
        return row[0]
    ```

    </details>

- 🅱️[**Longest Common Subsequence**](https://leetcode.com/problems/longest-common-subsequence/?md)⛅
  [💡](https://www.youtube.com/watch?v=Ua0GhsJSlWM)

  - `text1 = "abcde", text2 = "ace"` => `3` ("ace")
  - dp/recursive, add one and increase both if equal, else get the max of i+1 and j+1
  - <details>
      <summary>O(m*n) time, O(m*n)</summary>

    ```python
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        memo = {}
        i, j = 0, 0
        def recursive(i, j):
            if (i, j) in memo:
                return memo[i, j]
            if i == len(text1) or j == len(text2):
                return 0
            if text1[i] == text2[j]:
                memo[i, j] = 1 + recursive(i+1, j+1)
            else:
                memo[i, j] = max(recursive(i+1, j), recursive(i, j+1))
            return memo[i, j]
        return recursive(0, 0)
    ```

    </details>

- 🗿[**Best Time to Buy and Sell Stock with Cooldown**](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/?md)⛅:
  [💡](https://www.youtube.com/watch?v=I7j0F7AHpb8)

  - `prices = [1,2,3,0,2]` => `3` (buy at 1, sell at 3, buy at 0, sell at 2)
  - cache[i, can_buy], handle cooldown, can_buy and not can_buy
  - O(n) time, O(n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Coin Change 2**](https://leetcode.com/problems/coin-change-2/?md)⛅:
  [💡](https://www.youtube.com/watch?v=Mjy4hd2xgrs)

  - `amount = 5, coins = [1, 2, 5]` => `4`
  - Unbounded Knapsack, dfs(i, amount) = dfs(i+1, amount) + dfs(i, amount-coins[i])
  - O(n\*amount) time, O(n\*amount) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Target Sum**](https://leetcode.com/problems/target-sum/?md)⛅:
  [💡](https://www.youtube.com/watch?v=g0npyaQtAQM)

  - assign + or - `nums = [1, 1, 1, 1, 1], target = 3` => `5`
  - 0/1 Knapsack, O(2^n) -> O(n\*sum(nums)))
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Interleaving String**](https://leetcode.com/problems/interleaving-string/?md)⛅:
  [💡](https://www.youtube.com/watch?v=3Rw3p9LrgvE)

  - `s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbc`
  - check lengths, `if i < len(s1) and s1[i] == s3[i+j] and dfs(i+1, j): True`
  - O(n\*m) time, O(n\*m) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿 🏢[**Longest Increasing Path in a Matrix**](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/?md)⛈️:
  [💡](https://www.youtube.com/watch?v=wCc_nd-GiEc)

  - `matrix = [[9,9,4],[6,6,8],[2,1,1]]` => `4` (1, 2, 6, 9)
  - `result = max(result, 1+dfs(ii, jj))`, dfs every cell
  - O(m\*n) time, O(m\*n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Distinct Subsequences**](https://leetcode.com/problems/distinct-subsequences/?md)⛈️:
  [💡](https://www.youtube.com/watch?v=-RDzMJ33nx8)

  - `s = "rabbbit", t = "rabbit"` => `3` (rabbbit, rabbbit, rabbbit)
  - `if same: dfs(i+1, j+1) + dfs(i+1, j)` else `dfs(i+1, j)`
  - O(n\*m) time, O(n\*m) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Edit Distance**](https://leetcode.com/problems/edit-distance/?md)⛈️:
  [💡](https://leetcode.com/problems/edit-distance/solutions/159295/python-solutions-and-intuition/?orderBy=most_votes)

  - `word1 = "horse", word2 = "ros"` => `3`
  - lru_cache, `dp(len(s1), len(s2))`
  - O(m\*n) time, O(m\*n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Burst Balloons**](https://leetcode.com/problems/burst-balloons/?md)⛈️:
  [💡](https://www.youtube.com/watch?v=VFskby7lUbw)

  - `nums = [3,1,5,8]` => `167` (3\*1\*5 + 3\*5\*8 + 1\*3\*8 + 1\*8\*1)
  - `nums = [1] + nums + [1]`, `dfs(1, lens(nums)-2)`
  - O(n^3) time, O(n^2) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Regular Expression Matching**](https://leetcode.com/problems/regular-expression-matching/?md)⛈️:
  [💡](https://www.youtube.com/watch?v=HAA8mgxlov8)

  - `s = "aa", p = "a"` => `false`
  - `match = s[i] == p[j] or p[j] == '.'`, handle `*`, recursive
  - O(m\*n) time, O(m\*n) space (with cache)
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

## Greedy

---

- 🅱️🏢[**Maximum Subarray**](https://leetcode.com/problems/maximum-subarray/?md)⛅:
  [💡](https://www.youtube.com/watch?v=5WZl3MMT0Eg)

  - `nums = [-2,1,-3,4,-1,2,1,-5,4]` => `6` (`[4,-1,2,1]`)
  - `current_sum = max(current_sum+n, n)`, Kadane.
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def maxSubArray(self, nums: List[int]) -> int:
        result = nums[0]
        cur_sum = 0
        for n in nums:
            cur_sum = max(cur_sum+n, n)
            result = max(result, cur_sum)
        return result
    ```

    </details>

- 🅱️[**Jump Game**](https://leetcode.com/problems/jump-game/?md)⛅:
  [💡](https://www.youtube.com/watch?v=Yan0cv2cLy8)

  - `nums = [2,3,1,1,4]` => `true` (can reach last index)
  - start from end, `if i + nums[i] >= target: target = i`.
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def canJump(self, nums: List[int]) -> bool:
        target = len(nums)-1
        for i in range(len(nums)-2, -1, -1):
            if i + nums[i] >= target:
                target = i
        return target == 0
    ```

    </details>

- 🗿[**Jump Game II**](https://leetcode.com/problems/jump-game-ii/?md)⛅:
  [💡](https://www.youtube.com/watch?v=dJ7sWiOoK7g)

  - `nums = [2,3,1,1,4]` => `2` (jump 1 step from index 0 to 1, then 3 steps to the last index)
  - l = r = 0, farthest, l = r+1, r = farthest, count += 1
  - O(n) time, O(1) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Gas Station**](https://leetcode.com/problems/gas-station/?md)⛅:
  [💡](https://www.youtube.com/watch?v=lJwbPZGo05A)

  - `gas = [1,2,3,4,5], cost = [3,4,5,1,2]` => `3` (start at index 3)
  - check `sum(gas) >= sum(cost)`, if total is negative, reset and start from next index.
  - O(n) time, O(1) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Hand of Straights**](https://leetcode.com/problems/hand-of-straights/?md)⛅:
  [💡](https://www.youtube.com/watch?v=amnrMCVd2YI)

  - `hand = [1,2,3,6,2,3,4,7,8], W = 3` => `true`
  - counter, iterate keys
  - O(n+klogk) time, O(n) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Merge Triplets to Form Target Triplet**](https://leetcode.com/problems/merge-triplets-to-form-target-triplet/?md)⛅:
  [💡](https://www.youtube.com/watch?v=kShkQLQZ9K4)

  - `triplets = [[2,5,3],[1,8,4],[1,7,5]], target = [2,7,5]` => `true`
  - `if any(triplet[i] > target[i] for i in range(3)):` continue, update found[i]
  - O(n) time, O(1) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Partition Labels**](https://leetcode.com/problems/partition-labels/?md)⛅
  [💡](https://www.youtube.com/watch?v=B7m8UmZE-vw)

  - `s = "ababcbacadefegdehijhklij"` => `[9,7,8]`
  - last_idx = {}, end = max(idx, last_idx[c]), if idx == end, add to result.
  - O(n) time, O(1) space
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Valid Parenthesis String**](https://leetcode.com/problems/valid-parenthesis-string/?md)⛅:
  [💡](https://www.youtube.com/watch?v=QhPdNS143Qg)

  - `s = "(*)"` => `true`
  - left_min, left_max, `if l_max < 0: return False`, `if l_min < 0: l_min = 0`.
  - O(3^n)->O(n³)->O()
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

## Intervals

---

- 🅱️🏢[**Insert Interval**](https://leetcode.com/problems/insert-interval/?md)⛅:
  [💡](https://www.youtube.com/watch?v=A8NUOmlwOlM)

  - `intervals = [[1,3],[6,9]], newInterval = [2,5]` => `[[1,5],[6,9]]` (sorted)
  - `if newInterval[1] < interval[0]`, `if newInterval[0] > interval[1]`, else merge
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        if not intervals:
            return [newInterval]

        res = []
        for i, interval in enumerate(intervals):
            if interval[1] < newInterval[0]:
                res.append(interval)
            elif newInterval[1] < interval[0]:
                res.append(newInterval)
                return res + intervals[i:]
            else:
                newInterval = [
                    min(newInterval[0], interval[0]),
                    max(newInterval[1], interval[1])
                ]
        res.append(newInterval)
        return res
    ```

    </details>

- 🅱️🏢[**Merge Intervals**](https://leetcode.com/problems/merge-intervals/?md)⛅:
  [💡](https://www.youtube.com/watch?v=44H3cEC2fFM)

  - `intervals = [[1,3],[2,6],[8,10],[15,18]]` => `[[1,6],[8,10],[15,18]]`
  - Sort by start, append to result and merge `if start <= result[-1][1]`
  - <details>
      <summary>O(nlogn) time, O(n) space</summary>

    ```python
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        result = []
        for start, end in sorted(intervals):
            if not result or start > result[-1][1]:
                result.append([start, end])
            else:
                result[-1][1] = max(result[-1][1], end)
        return result
    ```

    </details>

- 🅱️[**Non-overlapping Intervals**](https://leetcode.com/problems/non-overlapping-intervals/?md)⛅:
  [💡](https://www.youtube.com/watch?v=nONCGxWoUfM)

  - `intervals = [[1,2],[2,3],[3,4],[1,3]]` => `1` (number of intervals to remove)
  - sort, update if start >= prev.end, else increase count
  - <details>
      <summary>O(nlogn) time, O(1) space</summary>

    ```python
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        end = float("-inf")
        overlap = 0
        for s, e in sorted(intervals, key=lambda x: x[1]):
            if s >= end:
                end = e
            else:
                overlap += 1
        return overlap
    ```

    </details>

- 🅱️[**Meeting Rooms**](https://leetcode.com/problems/meeting-rooms/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=PaJxqZVPhbg)

  - `intervals = [(0,30),(5,10),(15,20)]` => `false`
  - sort, if start < prev.end, return false
  - <details>
      <summary>O(nlogn) time, O(n) space</summary>

    ```python
    def canAttendMeetings(self, intervals):
        intervals.sort(key=lambda i: i[0])

        for i in range(1, len(intervals)):
            i1 = intervals[i - 1]
            i2 = intervals[i]

            if i1[1] > i2[0]:
                return False
        return True
    ```

    </details>

- 🅱️🏢[**Meeting Rooms II**](https://leetcode.com/problems/meeting-rooms-ii/?md)⛅:
  [💡](https://www.youtube.com/watch?v=FdzJmTCVyJU)
  min number of conference rooms.

  - `intervals = [(0,30),(5,10),(15,20)]` => `2`
  - sorted starts and ends, s and e pointers, increment count `if start[s] < end[e]`
  - <details>
      <summary>O(nlogn) time, O(n) space</summary>

    ```python
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        time = []
        for start, end in intervals:
            time.append((start, 1))
            time.append((end, -1))

        time.sort(key=lambda x: (x[0], x[1]))

        count = 0
        max_count = 0
        for t in time:
            count += t[1]
            max_count = max(max_count, count)
        return max_count
    ```

    </details>

- 🗿[**Minimum Interval to Include Each Query**](https://leetcode.com/problems/minimum-interval-to-include-each-query/?md)⛈️:
  [💡](https://www.youtube.com/watch?v=5hQ5WWW5awQ)

  - `intervals = [[1,4],[2,4],[3,6],[4,4]], queries = [2,3,4,5]` => `[3,-1,3,4]`
  - sort intervals & queries, heap (length, end), pop if end < query, dict[query] = length
  - <details>
      <summary>O(nlogn + qlogq) time, O(n+q) space</summary>

    ```python

    ```

    </details>

## Math & Geometry

---

- 🅱️[**Rotate Image**](https://leetcode.com/problems/rotate-image/?md)⛅:
  [💡](https://www.youtube.com/watch?v=fMSJSS7eO1w)

  - `matrix = [[1,2,3],[4,5,6],[7,8,9]]` => `[[7,4,1],[8,5,2],[9,6,3]]`
  - reverse rows, transpose
  - <details>
      <summary>O(cells) time, O(1) space</summary>

    ```python
    def rotate(self, matrix: List[List[int]]) -> None:
        l, r = 0, len(matrix) - 1
        while l < r:
          matrix[l], matrix[r] = matrix[r], matrix[l]
            l += 1
            r -= 1
        for i in range(len(matrix)):
            for j in range(i):
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    ```

    </details>

- 🅱️[**Spiral Matrix**](https://leetcode.com/problems/spiral-matrix/?md)⛅:
  [💡](https://www.youtube.com/watch?v=BJnMZNwUk1M)

  - `matrix = [[1,2,3],[4,5,6],[7,8,9]]` => `[1,2,3,6,9,8,7,4,5]`
  - 4 pointers, left, right, top, bottom, while l <= r and t <= b
  - <details>
      <summary>O(n^2) time, O(1) space</summary>

    ```python
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
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

    </details>

- 🅱️[**Set Matrix Zeroes**](https://leetcode.com/problems/set-matrix-zeroes/?md)⛅:
  [💡](https://www.youtube.com/watch?v=T41rL0L3Pnw)

  - `matrix = [[1,1,1],[1,0,1],[1,1,1]]` => `[[1,0,1],[0,0,0],[1,0,1]]`
  - use first row and column to store if row or column should be zeroed
  - <details>
      <summary>O(mn) time, O(m+n) space</summary>

    ```python
    def setZeroes(self, matrix: List[List[int]]) -> None:
        # O(1)
        ROWS, COLS = len(matrix), len(matrix[0])
        rowZero = False

        # determine which rows/cols need to be zero
        for r in range(ROWS):
            for c in range(COLS):
                if matrix[r][c] == 0:
                    matrix[0][c] = 0
                    if r > 0:
                        matrix[r][0] = 0
                    else:
                        rowZero = True

        for r in range(1, ROWS):
            for c in range(1, COLS):
                if matrix[0][c] == 0 or matrix[r][0] == 0:
                    matrix[r][c] = 0

        if matrix[0][0] == 0:
            for r in range(ROWS):
                matrix[r][0] = 0

        if rowZero:
            for c in range(COLS):
                matrix[0][c] = 0
    ```

    </details>

- 🗿 🏢[**Happy Number**](https://leetcode.com/problems/happy-number/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=ljz85bxOYJ0)

  - `n = 19` => `true` (`12 + 92 = 82 |...| 12 + 02 + 02 = 1`)
  - Hashset seen or Floyd's cycle detection
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

- 🗿[**Plus One**](https://leetcode.com/problems/plus-one/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=jIaA8boiG1s)

  - `digits = [1,2,3]` => `[1,2,4]`
  - carry = (digits[i]+carry) // 10, return [1]+result if carry
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Pow(x, n)**](https://leetcode.com/problems/powx-n/?md)⛅:
  [💡](https://www.youtube.com/watch?v=g9YQyYi4IQQ)

  - `x = 2.00000, n = 10` => `1024.00000`
  - `helper(x, abs(n))`, recursive helper
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

- 🗿[**Multiply Strings**](https://leetcode.com/problems/multiply-strings/?md)⛅:
  [💡](https://www.youtube.com/watch?v=1vZswirL8Y8)

  - `num1 = "2", num2 = "3"` => `"6"`
  - Manual multiplication, reverse, for in for
  - <details>
      <summary>O(nm) time, O(n+m) space</summary>

    ```python

    ```

    </details>

- 🗿 🏢[**Detect Squares**](https://leetcode.com/problems/detect-squares/?md)⛅:
  [💡](https://www.youtube.com/watch?v=bahebearrDc)

  - `points = [[3,10],[11,5],[11,10]]` => `[true,false,true]`
  - `res += self.points_count[x, py] * self.points_count[px, y]`
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python

    ```

    </details>

## Bit Manipulation (rare)

---

[Summary](http://leetcode.com/problems/sum-of-two-integers/discuss/84278/A-summary%3A-how-to-use-bit-manipulation-to-solve-problems-easily-and-efficiently)

<details>
  <summary>Click to expand</summary>

- 🗿[**Single Number**](https://leetcode.com/problems/single-number/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=qMPX1AOa83k)

  - `nums = [2,2,1]` => `1`
  - result ^= n
  - O(n) time, O(1) space
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

- 🅱️[**Number of 1 Bits**](https://leetcode.com/problems/number-of-1-bits/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=5Km3utixwZs)

  - `n = 11` => `3` (1011)
  - either `res += n & 1` and `n >> 1` or just `n = n & (n-1)` and `res += 1`
  - O(logn) time, O(1) space
  - <details>
      <summary>...</summary>

    ```python
    def hammingWeight(self, n: int) -> int:
        res = 0
        while n:
            n &= n - 1
            res += 1
        return res
    ```

    </details>

- 🅱️[**Counting Bits**](https://leetcode.com/problems/counting-bits/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=RyBM56RIWrM)

  - `n = 2` => `[0,1,1]`
  - `if offset * 2 == i: offset *= i`, `dp[i] = dp[i-offset] + 1`
  - <details>
      <summary>O(n) time, O(n) space</summary>

    ```python
    def countBits(self, n: int) -> List[int]:
        dp = [0] * (n + 1)
        offset = 1

        for i in range(1, n + 1):
            if offset * 2 == i:
                offset = i
            dp[i] = 1 + dp[i - offset]
        return dp
    ```

    </details>

- 🅱️[**Reverse Bits**](https://leetcode.com/problems/reverse-bits/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=UcoN6UjAI64)

  - `00000000000000000000000000000110` => `01100000000000000000000000000000`
  - `for i in range(32): bit = (n >> i) & 1`, `res |= bit << (31-i)`
  - <details>
      <summary>O(1) time, O(1) space (32 bit)</summary>

    ```python
    def reverseBits(self, n: int) -> int:
        res = 0
        for i in range(32):
            bit = (n >> i) & 1
            res += (bit << (31 - i))
        return res
    ```

    </details>

- 🅱️[**Missing Number**](https://leetcode.com/problems/missing-number/?ez)☀️:
  [💡](https://www.youtube.com/watch?v=WnPLSRLSANE)

  - `nums = [3,0,1]` => `2`
  - `for i, n in enumerate(nums): res ^= n; res ^= i+1`
  - <details>
      <summary>O(n) time, O(1) space</summary>

    ```python
    def missingNumber(self, nums: List[int]) -> int:
        res = len(nums)

        for i in range(len(nums)):
            res += i - nums[i]
        return res
    ```

    </details>

- 🅱️[**Sum of Two Integers**](https://leetcode.com/problems/sum-of-two-integers/?md)⛅:
  [💡](https://www.youtube.com/watch?v=gVUrDV4tZfY)

  - `a = 1, b = 2` => `3`
  - `return a if b == 0 else getSum(a^b, (a&b)<<1)`
  - <details>
      <summary>...</summary>

    ```python
    def getSum(self, a: int, b: int) -> int:
        def add(a, b):
            if not a or not b:
                return a or b
            return add(a ^ b, (a & b) << 1)

        if a * b < 0:  # assume a < 0, b > 0
            if a > 0:
                return self.getSum(b, a)
            if add(~a, 1) == b:  # -a == b
                return 0
            if add(~a, 1) < b:  # -a < b
                return add(~add(add(~a, 1), add(~b, 1)), 1)  # -add(-a, -b)

        return add(a, b)  # a*b >= 0 or (-a) > b > 0
    ```

    </details>

- 🗿[**Reverse Integer**](https://leetcode.com/problems/reverse-integer/?md)⛅:
  [💡](https://www.youtube.com/watch?v=HAgLH58IgJQ)

  - `x = 123` => `321`
  - TODO
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

</details>

## Extra

---

**Sorting**:

- ...

**Easy**:

- 🏢[**Logger Rate Limiter**](https://leetcode.com/problems/logger-rate-limiter/?md):
  [💡](https://gist.github.com/kuntalchandra/7822e388e0d2d78ec27f566266584b49)

  - `shouldPrintMessage(1, "foo"), shouldPrintMessage(3, "foo")` => `true, false`
  - Hashmap `{message: timestamp}`, `if self.cache[message] + 10 > timestamp`
  - O(1) time, O(n) space
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

- 🏢[**First Bad Version**](https://leetcode.com/problems/first-bad-version/?ez):
  [💡](https://leetcode.com/problems/first-bad-version/solutions/71324/python-understand-easily-from-binary-search-idea/?orderBy=most_votes)

  - `n = 5`, `isBadVersion(3) = false`, `isBadVersion(4) = true`
  - binary search.
  - O(logn) time, O(1) space
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

- 🏢[**Longest Common Prefix**](https://leetcode.com/problems/longest-common-prefix/?ez):
  [💡](https://www.youtube.com/watch?v=0sWShKIJoo4)

  - `strs = ["flower","flow","flight"]` => `"fl"`
  - iterate first word and compare
  - O(n) time, O(1) space
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

- 🏢[**Tic tac toe**](https://leetcode.com/problems/find-winner-on-a-tic-tac-toe-game/):
  [💡](https://leetcode.com/problems/find-winner-on-a-tic-tac-toe-game/solutions/1429722/find-winner-on-a-tic-tac-toe-game/)

  - `moves = [[0,0],[2,0],[1,1],[2,1],[2,2]]` => `"A"`
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

- 🏢[**Palindrome Number**](https://leetcode.com/problems/palindrome-number/?ez):
  [💡](https://www.youtube.com/watch?v=yubRKwixN-U)

  - `x = 121` => `true`
  - calculate divider, use mod
  - O(logn) time, O(1) space
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

- 🏢[**Roman to Integer**](https://leetcode.com/problems/roman-to-integer/?ez):
  [💡](https://www.youtube.com/watch?v=3jdxYj3DD98)

  - `s = "III"` => `3`
  - iterate, roman_dict, if roman[i] < roman[i+1]: res -= roman[i]
  - O(n) time, O(1) space
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

**Medium**:

- 🏢[**Range Sum Query - Mutable**](https://leetcode.com/problems/range-sum-query-mutable/description/):
  [💡](https://leetcode.com/problems/range-sum-query-mutable/solutions/75784/python-well-commented-solution-using-segment-trees/?orderBy=most_votes)

  - `update(i, val)`, `sumRange(i, j)`
  - Segment tree
  - O(logn) time, O(n) space
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

- 🏢[**Subarray Sum Equals K**](https://leetcode.com/problems/subarray-sum-equals-k/):
  [💡](https://youtu.be/fFVZt-6sgyo)

  - `nums = [1,1,1], k = 2` => `2` ([1, 1] and [1, 1])
  - prefix sum, if increase by k, found (subarray in between), initialize `d[0] = 1`
  - O(n) time, O(n) space
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

- 🏢[**Find original array from doubled array**](https://leetcode.com/problems/find-original-array-from-doubled-array/?md):
  [💡](https://leetcode.com/problems/find-original-array-from-doubled-array/solutions/1470959/java-c-python-match-from-the-smallest-or-biggest-100/)

  - `changed = [1,3,4,2,6,8]` => `[1,3,4]`
  - iterate sorted count, `if count[2*x] >= count[x]`, handle 0, `cnt[2*x] -= cnt[x]`
  - O(n + mlogm) time, O(m) space (m = unique elements)
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

- 🏢[**Decode String**](https://leetcode.com/problems/decode-string/?md):
  [💡](https://youtu.be/qB0zZpBJlh8)

  - `s = "3[a]2[bc]"` => `"aaabcbc"`
  - append to stack `if != ']'`, pop and multiply
  - O(n_output) time, O(n_output) space
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

- 🏢[**My Calendar I**](https://leetcode.com/problems/my-calendar-i/):
  [💡](https://www.youtube.com/watch?v=2SjzRBfXeN0)

  - `book(start, end)`, `book(10, 20)`, `book(15, 25)`, `book(20, 30)`
  - List (deque) + binary search
  - O(n) time, O(n) space
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

- 🏢[**Random Pick with Weight**](https://leetcode.com/problems/random-pick-with-weight/?md):
  [💡](https://leetcode.com/problems/random-pick-with-weight/solutions/154044/java-accumulated-freq-sum-binary-search/?orderBy=most_votes)

  - `[1,3]`, `pickIndex()` => `0` with 25% probability, `1` with 75% probability
  - binary search (random (1, total)) on the prefix sum.
  - O(n) time, O(n) space
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

- 🏢[**Number of matching subsequences**](https://leetcode.com/problems/number-of-matching-subsequences/?md):
  [💡](https://leetcode.com/problems/number-of-matching-subsequences/solutions/117634/efficient-and-simple-go-through-words-in-parallel-with-explanation/)

  - `s = "abcde", words = ["a","bb","acd","ace"]` => `3`
  - Hashmap `{char: [(word_idx, current_idx)]}`
  - O(s.length+sumcharwords) time, O(#words) space
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

- 🏢[**Longest String Chain**](https://leetcode.com/problems/longest-string-chain/?md):
  [💡](https://leetcode.com/problems/longest-string-chain/solutions/2153007/c-python-simple-solution-w-explanation-dp/?orderBy=most_votes)

  - `words = ["a","b","ba","bca","bda","bdca"]` => `4` (a, b, ba, bda)
  - start with shortest, for each word, check if word[:i]+word[i+1:] in dp
  - O(nlog(n) + nll) time, O(ns) space
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

- 🏢[**Step-By-Step Directions From a Binary Tree Node to Another**](https://leetcode.com/problems/step-by-step-directions-from-a-binary-tree-node-to-another/?md):
  [💡]

  - TODO
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

- 🏢[**The Number of Weak Characters in the Game**](https://leetcode.com/problems/the-number-of-weak-characters-in-the-game/?md):
  [💡](https://www.youtube.com/watch?v=9Y4ZQZ0YQ0o)

  - `properties = [[5,5],[6,3],[3,6]]` => `0`
  - sort by `-x[0],x[1]`, then iterate and keep max y
  - O(nlogn) time, O(1) space
  - <details>
      <summary>...</summary>

    ```python

    ```

    </details>

- 🏢[**Maximum Points You Can Obtain from Cards**](https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/?md):
  [💡](https://www.youtube.com/watch?v=TsA4vbtfCvo)
  - `cardPoints = [1,2,3,4,5,6,1], k = 3` => `12`
  - Sliding window, minimum subarray of size n-k.
  - O(n) time, O(1) space
- 🏢[**Stock Price Fluctuation**](https://leetcode.com/problems/stock-price-fluctuation/?md):
  [💡](https://leetcode.com/problems/stock-price-fluctuation/solutions/1513293/python-clean-2-heaps-commented-code/?orderBy=most_votes)
  - `update(int timestamp, int price)`, `current()`, `maximum()`, `minimum()`
  - 2 heaps, timestamps[time, price], self.highest_timestamp.
- 🏢[**Find Leaves of Binary Tree**](https://leetcode.com/problems/find-leaves-of-binary-tree/?md):
  [💡]
  - dfs, post-order, return layer.
- 🏢[**Snapshot Array**](https://leetcode.com/problems/snapshot-array/?md):
  [💡](https://leetcode.com/problems/snapshot-array/solutions/350562/java-python-binary-search/?orderBy=most_votes)
  - `SnapshotArray(int length)`, `set(index, val)`, `snap()`, `get(index, snap_id)`
  - Dict[int, array], binary search on the list of snapshots.
- 🏢[**Minimum Time Difference**](https://leetcode.com/problems/minimum-time-difference/?md):
  [💡](https://leetcode.com/problems/minimum-time-difference/solutions/100637/python-straightforward-with-explanation/comments/104667)
  - `timePoints = ["23:59","00:00"]` => `1`
  - map to minutes & sort, `(time[i]-time[i-1])%(24*60)`
  - O(nlogn) time, O(n) space
- 🏢[**Find All Possible Recipes from Given Supplies**](https://leetcode.com/problems/find-all-possible-recipes-from-given-supplies/description/?md):
  [💡](https://leetcode.com/discuss/interview-question/124658/Find-All-Possible-Recipes-from-Given-Supplies/127000)
  - TODO
  - ...
- 🏢https://leetcode.com/problems/battleships-in-a-board/?md
- 🏢https://leetcode.com/problems/find-and-replace-in-string/?md
- 🏢https://leetcode.com/problems/the-earliest-moment-when-everyone-become-friends/?md
- 🏢https://leetcode.com/problems/swap-adjacent-in-lr-string/?md
- 🏢https://leetcode.com/problems/sort-integers-by-the-power-value/?md
- 🏢https://leetcode.com/problems/maximum-number-of-points-with-cost/?md
- 🏢https://leetcode.com/problems/detonate-the-maximum-bombs/?md
- 🏢https://leetcode.com/problems/filling-bookcase-shelves/?md
- 🏢https://leetcode.com/problems/shortest-way-to-form-string/?md
- 🏢https://leetcode.com/problems/rle-iterator/?md
- 🏢https://leetcode.com/problems/check-if-word-can-be-placed-in-crossword/?md
- 🏢https://leetcode.com/problems/sentence-screen-fitting/?md
- 🏢https://leetcode.com/problems/parallel-courses/?md
- 🏢https://leetcode.com/problems/longest-line-of-consecutive-one-in-matrix/?md
- 🏢https://leetcode.com/problems/find-duplicate-subtrees/?md
- 🏢https://leetcode.com/problems/bulls-and-cows/?md
- 🏢https://leetcode.com/problems/time-needed-to-inform-all-employees/?md

**Hard**:

- 🏢[**Race Car**](https://leetcode.com/problems/race-car/?hd):
  [💡](https://leetcode.com/problems/race-car/solutions/124326/summary-of-the-bfs-and-dp-solutions-with-intuitive-explanation/)
  - TODO
  - ...
- 🏢[**Text Justification**](https://leetcode.com/problems/text-justification/?hd):
  [💡](https://leetcode.com/problems/text-justification/solutions/24891/concise-python-solution-10-lines/?orderBy=most_votes)
  - `words = ["This", "is", "an", "example", "of", "text", "justification."] w = 16` => `["This is an", "example of text", "justification. "]`
  - `if num_of_letters + len(w) + len(cur) > maxWidth:`, round robin
- 🏢[**Shortest Path in a Grid with Obstacles Elimination**](https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/?hd):
  [💡](https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/solutions/451787/python-o-m-n-k-bfs-solution-with-explanation/?orderBy=most_votes)
  - TODO
  - ...
- 🏢[**Poor Pigs**](https://leetcode.com/problems/poor-pigs/?hd):
  [💡](https://www.youtube.com/watch?v=6Z0rZ1J3Z8E)
  - `buckets = 1000, minutesToDie = 15, minutesToTest = 60` => `5`
  - TODO
  - ...
- 🏢https://leetcode.com/problems/maximum-number-of-visible-points/?hd
- 🏢[**Range Module**](https://leetcode.com/problems/range-module/?hd)
  [💡](https://www.youtube.com/watch?v=QhPdNS143Qg)
  - TODO
  - ...
- 🏢https://leetcode.com/problems/robot-room-cleaner/?hd
- 🏢https://leetcode.com/problems/employee-free-time/?hd
- 🏢https://leetcode.com/problems/expression-add-operators/?hd
- 🏢[**Student Attendance Record II**](https://leetcode.com/problems/student-attendance-record-ii/?hd):
  Fewer than 2 A, no 3 or more consecutive L.
  [💡]
  - `n = 2` => `8` ("PP", "AP", "PA", "LP", "PL", "AL", "LA", "LL")
- 🏢https://leetcode.com/problems/meeting-rooms-iii/?hd
- 🏢https://leetcode.com/problems/amount-of-new-area-painted-each-day/?hd
- 🏢https://leetcode.com/problems/number-of-atoms/?hd
- 🏢https://leetcode.com/problems/number-of-good-paths/?hd
- 🏢https://leetcode.com/problems/guess-the-word/?hd
- 🏢https://leetcode.com/problems/shortest-distance-from-all-buildings/?hd
- 🏢https://leetcode.com/problems/maximum-and-sum-of-array/?hd
- 🏢https://leetcode.com/problems/sum-of-prefix-scores-of-strings/?hd
- 🏢https://leetcode.com/problems/basic-calculator/

---

- [**Shortest Path in Binary Matrix**](https://leetcode.com/problems/shortest-path-in-binary-matrix/):
  A\*.
- [**Find the Index of the First Occurrence in a String**](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/):
  [💡]
  - [KMP](https://www.youtube.com/watch?v=GTJr8OvyEVQ) pattern matching, O(m+n).
- [**Maximum Length of Repeated Subarray**](https://leetcode.com/problems/maximum-length-of-repeated-subarray/):
  Rabin–Karp algorithm / Rolling Hash.
- [**Longest Duplicate Substring**](https://leetcode.com/problems/longest-duplicate-substring/) / [s](https://leetcode.com/problems/longest-duplicate-substring/solutions/695029/python-binary-search-o-n-log-n-average-with-rabin-karp-explained/):
  Rabin Karp + Binary Search.
- 🦠[**Close strings**](https://leetcode.com/problems/determine-if-two-strings-are-close):
  [💡](https://leetcode.com/problems/determine-if-two-strings-are-close/solutions/1029064/python-oneliner-with-counter-explained/?orderBy=most_votes)
  - `set(w1) == set(w2) and Counter(Counter(w1).values()) == Counter(Counter(w2).values()`
- [**String Compression**](https://leetcode.com/problems/string-compression/):
  2 pointers, slow and fast.

- [**Middle of the Linked List**](https://leetcode.com/problems/middle-of-the-linked-list/):
  [💡]()
  - `1->2->3->4->5` => `3`
  - slow = fast = head, while fast and fast.next: ..., return slow
  - O(n) time, O(1) space
- [**Linked List Cycle II**](https://leetcode.com/problems/linked-list-cycle-ii/):
  [💡](https://leetcode.com/problems/linked-list-cycle-ii/solutions/1701128/c-java-python-slow-and-fast-image-explanation-beginner-friendly/?orderBy=most_votes)
  - `head = [3,2,0,-4], -4 -> 2` => `2`
  - dist(intersect, cycle) == dist(head, cycle)
  - O(n) time, O(1) space
- [**Sudoku Solver**](https://leetcode.com/problems/sudoku-solver/): `if board[3 * (i // 3) + k // 3][ 3 * (j // 3) + k % 3] == n:`

### Links

- [Cracking the coding interview](https://www.amazon.com/Cracking-Coding-Interview-Programming-Questions/dp/0984782850)
- [Neetcode](https://neetcode.io/practice)
- [Leetcode](https://leetcode.com/)
- [Project Euler](https://projecteuler.net/)
- [HackerRank](https://www.hackerrank.com/)
