Here is the converted cheat sheet in clean, well-structured **`README.md`** format.

---

# Technical Interview Quick Reference & Cheat Sheet

> **Before Interview Note**
> _Date:_ Tuesday, May 19, 2026

---

## Table of Contents

- [Algorithm Design Techniques](https://www.google.com/search?q=%23algorithm-design-techniques)
- [Data Structures & Patterns](https://www.google.com/search?q=%23data-structures--patterns)
- [Recursion & Backtracking Patterns](https://www.google.com/search?q=%23recursion--backtracking-patterns)
- [Dynamic Programming (DP)](https://www.google.com/search?q=%23dynamic-programming-dp)
- [Trees](https://www.google.com/search?q=%23trees)
- [Graphs & Advanced Data Structures](https://www.google.com/search?q=%23graphs--advanced-data-structures)
- [Monotonic Stack Patterns](https://www.google.com/search?q=%23monotonic-stack-patterns)

---

## Algorithm Design Techniques

| Technique               | Key Indicators / Keywords                                 | Pattern & Strategy                                                                                                                                                                                                                  |
| ----------------------- | --------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **2 Pointer**           | Axis parallel, sorted arrays, 2 sum                       | Start from both ends or move together; use a pure `while` loop.                                                                                                                                                                     |
| **Slow & Fast Pointer** | Cycle detection, circular structures, finding middle node | $v_1 = 30\text{ km/h}, v_2 = 60\text{ km/h}$. `s = f = head` (finds 2nd middle).                                                                                                                                                    |
| **Sliding Window**      | Subarray/substring bounds, contiguous constraints         | **Slide & Undo Effect**: `right` expands window, `left` shrinks window. Maintain state using external math properties or hash maps.                                                                                                 |
| **Intervals**           | Overlapping ranges, line sweep, time slots                | Sort intervals $\rightarrow$ 2-pointer (winner strategy) / Min-Heap / HashMap + running count (line sweep).                                                                                                                         |
| **Prefix Sum**          | Subarray sum equals $K$, continuous range sum             | Store cumulative sums in HashMap. Remove extra front sub-segment: $\text{Target} = \text{CurrentSum} - \text{PrefixSum}$.                                                                                                           |
| **Greedy**              | Local optimal choice, swaps, min/max tracking             | Sorting & Heaps; traverse left-to-right or right-to-left. Suffix arrays, 2-pointer, or monotonic stack.                                                                                                                             |
| **Binary Search**       | Search space minimization, rotated arrays, answer range   | **Upper Bound:** `>`<br>**Lower Bound:** `>=`<br><br>**Rotated Array:** One side is always sorted.<br>**BS on Answer:** Find valid range and test feasibility with `mid`.<br>_Find largest minimized:_ `if feasible(mid): hi = mid` |
| **Matrices**            | Rotations, transformations, cell mappings                 | Map cell $(r, c)$ relative to original dimensions. E.g., **$90^\circ$ Clockwise Rotation:** $(r, c) \to (c, n - 1 - r)$.                                                                                                            |

---

## Data Structures & Patterns

### Linked List

- **Delete Node:** Skip reference $\rightarrow$ `head.next = head.next.next`
- **Insert at End:** `temp.next = newNode`
- **Reach $k$-th Node:** Iterate $k - 1$ times.
- **Recursion:** Directly reach the target node and manipulate pointers.

### Doubly Linked List

- **Insert at End:**

```python
temp.next = newNode
newNode.prev = temp

```

- **Insert in Between (4 Pointer Changes):**

```python
newNode.next = temp.next
newNode.prev = temp
temp.next.prev = newNode
temp.next = newNode

```

- **Delete in Between:**

```python
target.prev.next = target.next
target.next.prev = target.prev

```

### Hash Maps & Queues

- **HashMap Lookups:** Frequency counting / Two Sum lookups.
- **Anagram Grouping:**

```python
anagramMap[tuple(sorted(s))].append(s)

```

- **Deque:** Double-ended queue used for BFS and sliding window maximum/minimum problems (`dq = deque()`).

---

## Recursion & Backtracking Patterns

### 1. Pick / Skip Pattern (Subsets & Combinations)

```python
def util(idx, temp):
    if idx == N:
        result.append(list(temp))
        return

    # Pick
    temp.append(nums[idx])
    util(idx + 1, temp)

    # Skip
    temp.pop()
    util(idx + 1, temp)

N, result = len(nums), []
util(0, [])
return result

```

### 2. String Partitioning / Splitting

```python
def splitstring(idx, s, result):
    if idx == len(s):
        print(result)
        return

    for i in range(idx, len(s)):
        # Include substring s[idx : i + 1]
        result.append(s[idx : i + 1])
        splitstring(i + 1, s, result)
        result.pop()

splitstring(0, '199100199', [])

```

### 3. Permutations Pattern

```python
def f(used):
    if len(used) == len(arr):
        print(used)
        return

    # All elements can be chosen at any point based on path
    for a in arr:
        # Only unused elements in current path are allowed
        if a not in used:
            used.append(a)
            f(used)
            used.pop()

```

---

## Dynamic Programming (DP)

### 1D DP: Frog Jump / Minimal Cost

```python
def minimizeCost(index, arr, k, cache):
    if index == len(arr) - 1:
        return 0
    if index in cache:
        return cache[index]

    minEnergy = float('inf')
    # From every index, we can go to next k indices
    for i in range(1, k + 1):
        if index + i < len(arr):
            currentEnergy = abs(arr[index] - arr[index + i])
            furtherEnergyRequired = minimizeCost(index + i, arr, k, cache)
            minEnergy = min(minEnergy, currentEnergy + furtherEnergyRequired)

    cache[index] = minEnergy
    return minEnergy

```

### 2D DP: Dungeon Game (Grid Minimum Health)

```python
def calculateMinimumHP(self, dungeon: list[list[int]]) -> int:
    M, N = len(dungeon), len(dungeon[0])
    cache = {}

    def function(i, j):
        if i == M - 1 and j == N - 1:
            return 1 if dungeon[i][j] >= 0 else abs(dungeon[i][j]) + 1
        if i >= M or j >= N:
            return float('inf')
        if (i, j) in cache:
            return cache[(i, j)]

        down = function(i + 1, j)
        right = function(i, j + 1)

        # Minimum energy required to reach next cell
        need_from_prev_cell = min(down, right)
        need = need_from_prev_cell - dungeon[i][j]

        cache[(i, j)] = 1 if need <= 0 else need
        return cache[(i, j)]

    return function(0, 0)

```

### 3D DP: Cherry Pickup II (Simultaneous Traversal)

```python
def f(i, j1, j2, n, m, grid, cache):
    if j1 < 0 or j1 >= m or j2 < 0 or j2 >= m:
        return float('-inf')
    if i == n - 1:
        return grid[i][j1] if j1 == j2 else grid[i][j1] + grid[i][j2]
    if (i, j1, j2) in cache:
        return cache[(i, j1, j2)]

    maxi = float('-inf')
    # Try all direction combinations for robot 1 and robot 2 (-1, 0, 1)
    for di in range(-1, 2):
        for dj in range(-1, 2):
            ans = grid[i][j1] if j1 == j2 else grid[i][j1] + grid[i][j2]
            ans += f(i + 1, j1 + di, j2 + dj, n, m, grid, cache)
            maxi = max(maxi, ans)

    cache[(i, j1, j2)] = maxi
    return cache[(i, j1, j2)]

```

### LIS (Longest Increasing Subsequence)

```python
def f(i, prev, nums, cache):
    if i == len(nums):
        return 0
    if (i, prev) in cache:
        return cache[(i, prev)]

    # Skip current element
    skip = f(i + 1, prev, nums, cache)

    # Pick current element if condition satisfied
    pick = 0
    if prev == -1 or nums[i] > nums[prev]:
        pick = 1 + f(i + 1, i, nums, cache)

    cache[(i, prev)] = max(pick, skip)
    return cache[(i, prev)]

```

### LCS (Longest Common Subsequence)

```python
def f(i, j, text1, text2, cache):
    if i == len(text1) or j == len(text2):
        return 0
    if (i, j) in cache:
        return cache[(i, j)]

    if text1[i] == text2[j]:
        cache[(i, j)] = 1 + f(i + 1, j + 1, text1, text2, cache)
    else:
        op1 = f(i + 1, j, text1, text2, cache)
        op2 = f(i, j + 1, text1, text2, cache)
        cache[(i, j)] = max(op1, op2)

    return cache[(i, j)]

```

### Longest Common Substring DP

```python
def longestCommonSubstring(str1: str, str2: str) -> int:
    n1, n2 = len(str1), len(str2)
    table = [[0] * (n2 + 1) for _ in range(n1 + 1)]
    lcs = 0

    for s1 in range(1, n1 + 1):
        for s2 in range(1, n2 + 1):
            if str1[s1 - 1] == str2[s2 - 1]:
                table[s1][s2] = 1 + table[s1 - 1][s2 - 1]
                lcs = max(lcs, table[s1][s2])
            else:
                table[s1][s2] = 0  # Reset on mismatch (consecutive property lost)

    return lcs

```

### Partition DP: Minimum Cost to Cut a Stick

```python
class Solution:
    def minCost(self, n: int, cuts: list[int]) -> int:
        cuts = [0] + sorted(cuts) + [n]
        cache = {}

        def f(i, j):
            if i > j:
                return 0
            if (i, j) in cache:
                return cache[(i, j)]

            maxi = float('inf')
            curcutcost = cuts[j + 1] - cuts[i - 1]

            for k in range(i, j + 1):
                cost = curcutcost + f(i, k - 1) + f(k + 1, j)
                maxi = min(maxi, cost)

            cache[(i, j)] = maxi
            return cache[(i, j)]

        return f(1, len(cuts) - 2)

```

### State DP: Best Time to Buy and Sell Stock with Cooldown

```python
def util(i, buy, prices, dp):
    if i >= len(prices):
        return 0
    if (i, buy) in dp:
        return dp[(i, buy)]

    if buy == 1:
        # State: Can Buy -> Max of (Skip today, Buy today and pay price)
        profit = max(util(i + 1, 1, prices, dp), -prices[i] + util(i + 1, 0, prices, dp))
    else:
        # State: Can Sell -> Max of (Skip today, Sell today and gain price + 1 day cooldown)
        profit = max(util(i + 1, 0, prices, dp), prices[i] + util(i + 2, 1, prices, dp))

    dp[(i, buy)] = profit
    return dp[(i, buy)]

# Start traversal at index 0 with ability to buy (buy = 1)

```

---

## Trees

### Breadth-First Search (BFS) — Level Order Traversal

- **Time Complexity:** $O(N)$

```python
from collections import deque

class Solution:
    def levelOrder(self, root):
        if not root:
            return []
        res, q = [], deque([root])

        while q:
            sze = len(q)
            temp = []
            for _ in range(sze):
                cur = q.popleft()
                temp.append(cur.val)
                if cur.left:
                    q.append(cur.left)
                if cur.right:
                    q.append(cur.right)
            res.append(temp)

        return res

```

### Depth-First Search (DFS)

- **Time Complexity:** $O(N)$

```python
def helper(current, visited, adj, result):
    if current in visited:
        return
    visited.add(current)
    result.append(current)
    for nxt in adj[current]:
        helper(nxt, visited, adj, result)

```

### Validate Binary Search Tree (BST)

```python
class Solution:
    def isValidBST(self, root) -> bool:
        prev = None

        def f(node):
            nonlocal prev
            if not node:
                return True
            if not f(node.left):
                return False
            if prev and prev.val >= node.val:
                return False
            prev = node
            return f(node.right)

        return f(root)

```

---

## Graphs & Advanced Data Structures

### Graph Representations

```python
# Unweighted Graph
def createGraph(V, edges):
    graph = {i: set() for i in range(V)}
    for u, v in edges:
        graph[u].add(v)
        graph[v].add(u)
    return graph

# Weighted Graph
def createWeightedGraph(V, edges):
    graph = {i: [] for i in range(V)}
    for u, v, w in edges:
        graph[u].append((v, w))
        graph[v].append((u, w))
    return graph

```

### Graph Traversal (DFS & BFS)

- **Time Complexity:** $O(V + E)$

```python
# DFS Traversal
def dfs(adj, V):
    visited, result = set(), []

    def f(u):
        if u in visited:
            return
        visited.add(u)
        result.append(u)
        for v in adj[u]:
            f(v)

    f(0)
    return result

# BFS Traversal
def bfs(adj):
    q = deque([0])
    seen = {0}
    result = []

    while q:
        u = q.popleft()
        result.append(u)
        for v in adj[u]:
            if v not in seen:
                seen.add(v)
                q.append(v)

    return result

```

### Trie Data Structure

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        node = self.root
        for ch in word:
            if ch not in node.children:
                node.children[ch] = TrieNode()
            node = node.children[ch]
        node.is_end = True

    def search(self, word: str) -> bool:
        node = self.root
        for ch in word:
            if ch not in node.children:
                return False
            node = node.children[ch]
        return node.is_end

```

### Disjoint Set Union (DSU with Rank)

- **Time Complexity:** $O(E \cdot \alpha(V))$

```python
class RankDSU:
    def __init__(self, n):
        self.parent = [i for i in range(n)]
        self.rank = [1] * n
        self.components = n

    def find(self, x: int) -> int:
        if x != self.parent[x]:
            self.parent[x] = self.find(self.parent[x])  # Path compression
        return self.parent[x]

    def union(self, x: int, y: int) -> bool:
        pX, pY = self.find(x), self.find(y)
        if pX == pY:
            return False
        if self.rank[pX] < self.rank[pY]:
            pX, pY = pY, pX
        self.parent[pY] = pX
        self.rank[pX] += self.rank[pY]
        self.components -= 1
        return True

```

### Shortest Path: Dijkstra's Algorithm

- **Time Complexity:** $O((V + E) \log V)$

```python
import heapq

def dijkstra(graph, V, src):
    d = [float('inf')] * V
    d[src] = 0
    pq = [(0, src)]  # (distance, node)

    while pq:
        curW, u = heapq.heappop(pq)
        if curW > d[u]:
            continue

        for v, w in graph[u]:
            newDist = curW + w
            if newDist < d[v]:
                d[v] = newDist
                heapq.heappush(pq, (newDist, v))

    return d

```

### Topological Sort: Kahn's Algorithm (BFS)

- **Time Complexity:** $O(V + E)$

```python
def canFinish(N: int, prerequisites: list[list[int]]) -> bool:
    inDegree = [0] * N
    graph = {i: set() for i in range(N)}

    for u, v in prerequisites:
        inDegree[u] += 1
        graph[v].add(u)

    q = deque([i for i in range(N) if inDegree[i] == 0])
    processed = 0

    while q:
        cur = q.popleft()
        processed += 1
        for nxt in graph[cur]:
            inDegree[nxt] -= 1
            if inDegree[nxt] == 0:
                q.append(nxt)

    return processed == N

```

### Minimum Spanning Tree: Prim's Algorithm

- **Time Complexity:** $O((V + E) \log V)$

```python
def spanningTree(V, graph):
    visited = set()
    pq = [(0, 0)]  # (weight, node)
    total_weight = 0

    while pq:
        wt, u = heapq.heappop(pq)
        if u in visited:
            continue

        visited.add(u)
        total_weight += wt

        for v, w in graph[u]:
            if v not in visited:
                heapq.heappush(pq, (w, v))

    return total_weight if len(visited) == V else -1

```

---

## Monotonic Stack Patterns

### Next Greater Element (NGE)

- **Strategy:** Iterate **Right-to-Left** (`n-1` down to `0`). Maintain a **decreasing stack**.

```python
def NGE(nums):
    n = len(nums)
    stack = []
    nge_map = {}

    for i in range(n - 1, -1, -1):
        current = nums[i]
        while stack and stack[-1] <= current:
            stack.pop()
        nge_map[current] = stack[-1] if stack else -1
        stack.append(current)

    return nge_map

```

### Previous Smaller Element (PSE)

- **Strategy:** Iterate **Left-to-Right** (`0` to `n-1`). Maintain an **increasing stack**.

```python
def PSE(nums):
    n = len(nums)
    stack = []
    pse_map = {}

    for i in range(n):
        current = nums[i]
        while stack and stack[-1] >= current:
            stack.pop()
        pse_map[current] = stack[-1] if stack else -1
        stack.append(current)

    return pse_map

```
