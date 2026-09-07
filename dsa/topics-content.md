# DSA Notes

## 1. Array Techniques

### Prefix Sum

1. Problem asks for count/length of subarray based on some mathematical condition like sum, divisibility, mod, vowel, binary xor etc.
2. Sliding window will not work for this problems because of –ve elements or some other condition.
3. T(n) = O(n); S(n) = O(n) due to map.

**Basic Idea**

2. Psumi – current running sum; psumj subarray stored in map.
3. If psumi-psumj = k which means [j...i] sum is k/ check psumi - k in map.
4. Map[0] = 1 if psumi-k = 0 then entire subarray is answer so count = 1
5. Map[0] = -1 if psumi-k = 0 which means entire subarray is answer. Length = I+1

### 2 Pointer

- pure while loop
- sorting → running from both end
- palindrome/reverse → either half matters (axial symmettry)

| | |
|---|---|
| C[I].lower() | Convert str/char to lower |
| C[I].isalnum() | Return True if char is alpha or number |
| C[I].isalpha() | Return True if char is alpha |
| C[I].isdigit() | Return True if char is digit |
| C[I].isnumeric() | Return True if char is digit |

### Interval / Line Sweep

Think in terms of number line and place variable accordingly.

| | |
|---|---|
| Line Sweep | Mark the events in number line and sweep it. |

**Template**

```python
while i < N:
    # ---interval------newinterval
    if not newInterval or intervals[i][1] < newInterval[0]:
        res.append(intervals[i])
     # ---newinterval------interval
    elif newInterval[1] < intervals[i][0]:
        res.append(newInterval)
        res.append(intervals[i])
        newInterval = None
    else:
        newInterval[0] = min(newInterval[0], intervals[i][0])
        newInterval[1] = max(newInterval[1], intervals[i][1])
    i += 1
```

**Line Sweep**

```python
brightness_range = [0] * (n+1)
for pos, ran in lights:
    start = max(0, pos - ran)
    end = min(n-1, pos+ran)
    # mark events
    brightness_range[start] += 1
    brightness_range[end+1] -= 1

sweep_line = count = 0
for i in range(n):
    sweep_line += brightness_range[i] #sweep
    if sweep_line >= requirement[i]:
        count += 1
```

## 2. Sliding Window

### Sliding Window

- Simple observation – consecutive windows overlap significantly
- No need to re-compute everything, shares k-1 elements with previous one
  - Account only for elements leaving and coming(undo to k-1)
- Expand (right += 1) until condition is violated & shrink (left += 1) until condition is restored.

**Why Sliding Window works ?**

Sliding window works best

a. when condition on window changes in one consistent direction(monotone) as window grows or shrinks.
b. Shrinking window can move from invalid state to valid or vice versa

**When to use?** Subarray/substring problems, longest/shortest/minimum/maximum/expand and shrink mental model

**When not to use?** Non contigious elements/no monotone property e.g. -ve numbers/elements can be rearranged/track all subarray not just one.

**Template**

```python
left = right = 0
while right < N:
    while voilated:
        left += 1 or move to index
    right += 1
```

**Finding Maximum**

```python
While condition_voilated:
    Shrink()
Result = max(Result, window_size)
```

**Finding Minimum**

```python
While condition_satisified:
    Result = min(Result, window_size) # since all small windows are answer
    Shrink()
```

**Exactly K Problem**

```
Exactly(k) = atmost(k) - atmost(k-1)
```

**Choosing Window State**

| Need | Store |
|---|---|
| Sum/product | Running Sum |
| Count frequency | HashMap / Counter |
| Character frequency | Counter / int[26] |
| Need max/min element inside window | Monotonic Queue(deque) |
| Need index positions | HashMap |
| Need duplicates check | HashSet |
| Need order of elements | Deque |
| Binary problems | Bits |
| 2 Heaps Pattern | median |

"Heaps don't track window expiration naturally; monotonic queue maintains the window's max/min candidates and removes expired elements efficiently."

**Formula**

`Count += (right-left+1)` "number of possible starting points for subarrays ending at current right".

All subarrays will include right

`n * (n+1) / 2` total number of subarrays in an array of size n.

### Sliding Window (Summary)

1. Simple Observation – consecutive windows overlap significantly so instead of recomputing, account only for elements leaving and coming.
2. Sliding window works best when condition on window changes in one consistent direction as window grows or shrinks.
3. Expand and shrink is common for all problem but what data structure we are going to use to track the state and update it matters.
4. Choosing Window State
   a. Running sum – sum/average
   b. Counter – Strings
   c. HashMap – when index matters
   d. Monotonic Stack/Queue - based on need
   e. Heaps
   f. Bits.

## 3. Pointers

### 2 Pointer

- pure while loop
- sorting → running from both end
- palindrome/reverse → either half matters (axial symmettry)

| | |
|---|---|
| C[I].lower() | Convert str/char to lower |
| C[I].isalnum() | Return True if char is alpha or number |
| C[I].isalpha() | Return True if char is alpha |
| C[I].isdigit() | Return True if char is digit |
| C[I].isnumeric() | Return True if char is digit |

## 4. Interval

Think in terms of number line and place variable accordingly.

| | |
|---|---|
| Line Sweep | Mark the events in number line and sweep it. |

**Template**

```python
while i < N:
    # ---interval------newinterval
    if not newInterval or intervals[i][1] < newInterval[0]:
        res.append(intervals[i])
     # ---newinterval------interval
    elif newInterval[1] < intervals[i][0]:
        res.append(newInterval)
        res.append(intervals[i])
        newInterval = None
    else:
        newInterval[0] = min(newInterval[0], intervals[i][0])
        newInterval[1] = max(newInterval[1], intervals[i][1])
    i += 1
```

**Line Sweep**

```python
brightness_range = [0] * (n+1)
for pos, ran in lights:
    start = max(0, pos - ran)
    end = min(n-1, pos+ran)
    # mark events
    brightness_range[start] += 1
    brightness_range[end+1] -= 1

sweep_line = count = 0
for i in range(n):
    sweep_line += brightness_range[i] #sweep
    if sweep_line >= requirement[i]:
        count += 1
```

## 5. Binary Search

- Binary search because of monotonicity that values are keep increasing

| Problem Type | Examples |
|---|---|
| Finding exact value | Search in sorted array, search in matrix |
| Finding boundary(Lower/Upper) | First/last occurrence, first bad version |
| Finding peak/valley | Find peak element, find minimum in rotated array |
| Search on answer | Koko eating bananas, capacity to ship packages |
| Optimization | Minimize maximum, maximize minimum |

For NGE-style problems, remember:

- bisect_left → first >=
- bisect_right → first > (after existing equal values)

When there is no such values – array length will be returned.

**Template**

```python
class Solution:
    def splitArray(self, nums: List[int], k: int) -> int:
        def canSplit(subarray_sum):
            total = 0
            count = 1
            for n in nums:
                total += n
                if total > subarray_sum:
                    count += 1
                    total = n
            return count <= k #possible answer

        l, r = max(nums), sum(nums)
        while l < r:
            m = (l+r)//2
            if canSplit(m):
                r = m#because even m is valid answer
            else:
                l = m+1
        return l
```

**Lower and Upper Bound**

a. Lower bound >=
b. Upper bound > (Strictly greater)

**Binary Search on Answer**

- Guess the answer → check if your guess is possible → adjust guess.
- If someone GIVES me the answer X, can I check if it is valid?
- find largest minimized → bs on value + lower bound
- bs on value → assume a value to be answer and try the value to check correctness.
- bs on value → find the valid range + apply value on some small logic
- minimized maximum/ absolute distance
- when the input data itself contain the answer

References:
- https://leetcode.com/problem-list/binary-search/
- https://cp-algorithms.com/num_methods/binary_search.html
- https://usaco.guide/silver/binary-search?lang=cpp
- https://codeforces.com/topic/146750/en1

## 6. Stacks

### Monotonic Stack

**What?** Stack where elements are either increasing or decreasing from bottom to top.

Before pushing current elements, pop elements which violate monotonicity

Keep only eligible candidates in stack (proof based pruning/fast loop up)

| | |
|---|---|
| Decision 1 | Direction left to right or right to left |
| Decision 2 | Need Min or Max; if Max needed remove all smaller before pushing current. |

**Monotonic Stack Example**

Direction and what we need min or max

- I want smaller – remove all larger because everybody needs small he is bigger than me which says I'm small, for people coming after me, I'm the answer. If somebody useless for me they are not useful for anybody.

That creates a monotonic stack invariant:
- Stack contains only "still useful" candidates. I.e. what I need max so keep only max's arranged in selectable order decreasing from top to bottom.

Without maintaining monotonic order, you keep many useless values and repeatedly re-check them, which drifts toward brute-force behavior.

Next Greater Element – I cannot predict the future so come from future (right to left)

Previous Greater Element – we know future, come from left to right.

Need Greater – remove all smaller and keep only greater in point of view of current.

Need Smaller - remove all larger and keep only smaller in point of view of current.

**Template**

```python
def next(self, price: int) -> int:
    # i want greater, remove small
    self.current_idx += 1
    while self.span_stack and self.span_stack[-1][0] <= price:
        self.span_stack.pop()
    # compare with last greater - the gap b/w 2 great is answer
    ans = self.current_idx - self.span_stack[-1][1] if self.span_stack else self.current_idx
    self.span_stack.append((price, self.current_idx))
    return ans
```

```
sort from front → back
for each element:
    calculate current value
    if current creates a new state:
        push
return len(stack)
```

## 7. Linkedlist

| | | |
|---|---|---|
| SLL Insert | Node.next = newNode | Attach with another node |
| SLL Delete | Node.next = Node.next.next | Skip the node |
| SLL Delete Tail | Need second last node & while current.next.next: | Stop before tail not at tail |
| Traversal | To stop at kth node, traverse k-1 as head already in first node; 1 index based : (1, k) stops at k -1; 0 index based : (0,k) stop at k-1 | One traverse always less because head count ignored and it takes 0 moves to reach head. |
| Recursive SLL Traversal | Reach the node itself and return the new node attached the chain. or None(in case of delete) | |
| Recursive DLL Traversal | Attaching previous needs care as current node in recursion stack might need something they called which may be not available now. | |
| DLL Traversal | Reach the node/position itself sine we have previous pointer unlike SLL. | Destroy/Modify the node with itself. |
| DLL Insert In-between (4 Pointer Change) | newnode.next = temp.next; newnode.prev = temp; temp.next.prev = newnode ; temp.next = newnode | Make changes to new node 1st Then make changes to nodes new node pointed.(always one next and one prev) |
| DLL Delete Inbetween(reach the Target node) - 2 pointer changes. | target.prev.next = target.next; target.next.prev = target.prev | Reach target itself. |
| DLL tail delete | Go to tail;tail.prev.next = None | Reach tail itself |
| Common Edge Cases | Empty list, One node, Deleting head, Deleting tail | |

Think In terms of number line

```
---PrevNode---CurrentNode---NextNode--
```

Always use dummy pointer and move dummy pointer as new insert nodes. Store head of dummy elsewhere

head pointer very important and always special case

| | |
|---|---|
| slow and fast pointer | 1. finding middle 1. 1st mid -> s=head, f=head.next 2. 2nd mid -> s=f=head 2 Pointer Winner Algorithm. |
| LRU | 1. DL 2. L with dummy head and tail 3. Add to head (itself handle tail) 4. Remove node with node reference 5. Remove tail (lru = self.tail.prev) — Constant time because we have direct nice reference |
| Reverse K group | 1. Find k the node (1,k) and break 2. Reverse node 3. Attach kthnode to prevGrouptail since it is new head. 4. Maintain prevGrouptail which will be current since Current is forwarded by reverse function. — Attach current to tail of previous groups tail because current now came at last; next will be current.next |
| LFU | 1. Extension of LRU 2. Each node knows its freq 3. Map<freq,DLL>, Map<key,Node> 4. When get or update happens frequency increases. — LRU extension with another map |
| Flatten a Multilevel Doubly Linked List | If child available make it dynamic, add next after all childs. Continue main thread as normal current = head. — Regular linked list creation, handle child ok recursive way; even child itself a LL. |

## 8. Trees

- DFS/BFS/BST
- Like DP start thinking as single node problem and replicate
- Check the dependency whether parent can short circuit or parent need something from child to decide.
- Subtree is different from path.

## 9. Greedy

Local choice leading to global answer like jump game. - smart sorting/heap/right scan/2 pointer

so i should select / design a greedy solution which works for the given problem, based on problem i should be greedy on asc, desc, anything else.

- take local best decision e.g. jump game (take max available), boats (pair s & b or just b)
- sorting & heaps often comes & swaps, traversing from right to left, left to right and keeping max/min

| If the problem says: | Think: |
|---|---|
| "maximize result after one action" | suffix max from right |
| "pair things optimally" | two pointers (min + max) |
| "remove K elements optimally" | monotonic stack |
| "schedule tasks / intervals" | sort by end time |
| "min total cost from repeated operations" | min heap |
| "maximize non-overlapping tasks" | earliest end time |
| "maximize distance / reachability" | farthest reach greedy |
| "min swaps / fix array digits" | right-scan + swap with best future |
| "minimize final number/string" | remove largest leftmost bad digit |

```python
arr.sort(key=lambda x: x[1], reverse=True)
```

**How to recognize a greedy problem**

Ask yourself:

a. Can I make one decision now without regretting it later?
b. Can I prove that this decision is always at least as good as any other?
c. After making that decision, does the remaining problem have the same structure?

If the answer is yes, a greedy solution may exist.

"What choice leaves me with the most flexibility for the remaining work?"

**Greedy thinking**

```python
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        #maximize number of childs
        #g[i] min they will accept.
        #sort cookie and student asc - if student cannot sat with
        current cookie he will never will - no wrong the cookie is waste it
        won't satifis any one.
        m, n = len(s), len(g)
        i = j = count = 0
        g.sort()
        s.sort()
        while i < m and j < n:
            if s[i] < g[j]:#waste cookie cannot satisfy anyone
                i += 1
            else:
                i += 1
                j += 1
                count += 1
        return count
```

## 10. Heaps

Heaps – Sorting and Heaps can together work very well to do handle different dimension of problem.

**Patterns**

| | |
|---|---|
| Sorting + Heap | Sorting fixes which is common for all. Max heap will be used to add and remove |
| 2 Heaps | Simulate sorting but mid we will have in mid. Make a mountain. |
| Greedy + Marginal Benefit / Delta Optimization | Add benefit to heap with normal value (p+1/q+1),p,q. If it comes as candidate then change the actual data. marginal_maxheap pa |
| Fixing the Maximum/Minimum Constraint | Sort which will set the common value then select k people apply then remove based on need min ot max. In hire k – remove high quality since we need minimum cost |
| 2402. Meeting Rooms III | Clean up finished on, current start is heapq.heapify(arr) / list.sort(reverse=True) |

Sheet Link

### 1. K Way Merge.

K-way merge is a heap pattern used when you need to merge K already sorted sequences into one sorted sequence efficiently.

The key idea:

Instead of comparing all K elements every time, use a min heap of size K.

**Example**

You have 3 sorted arrays:

```
A = [1, 4, 7]
B = [2, 5, 8]
C = [3, 6, 9]
```

Need:

```
[1,2,3,4,5,6,7,8,9]
```

**Naive approach**

Take smallest among current elements of all arrays:

```
compare 1,2,3 -> take 1
compare 4,2,3 -> take 2
compare 4,5,3 -> take 3
...
```

Each time you compare K elements:

Time: `O(N*K)` where N = total elements.

**Heap approach**

Keep only the current smallest candidate from each array.

Initial heap:

```
(1,A)
(2,B)
(3,C)
```

```
Heap:1
    / \
   2   3
```

Pop min: take 1

Now from A, insert next element: (4,A)

```
Heap:
    2
   / \
  3   4
```

Pop: take 2, insert 5

Continue.

**Why heap works?**

At any moment: heap contains K possible next smallest elements

The smallest among them is guaranteed to be the next answer.

**Complexity**

Total elements = N
Heap size = K
Each element:
- remove min → O(log K)
- insert next element → O(log K)

So: Time = O(N log K), Space = O(K)

**Common interview problems using K-way merge**

a. Merge K sorted linked lists
b. Find Kth smallest element in K sorted arrays
c. Smallest range covering elements from K lists
d. Merge K sorted files (external sorting)
e. Top K elements from multiple sorted streams

A good mental trigger:

"I have multiple sorted sources and need a global sorted result" → think K-way merge + heap.

**Pattern**

```
pop smallest
add next candidate from same source
```

**Top K Pattern**

```
# want max values - use minheap so you can remove smaller values
```

## 11. Graphs

Graph – tree with cycle; visit neighbors( can be adj list, matrix co-ordinates, map)

| Algorithm | Hint | Problem | Rev 1 |
|---|---|---|---|
| DFS | Visit neighbors deeper | Number of Islands | Just recursion on neighbor with visited array; vis can be checked in top or in for loop. |
| BFS | Visit neighbors level by level | 94. Rotting Oranges / 542. 01 Matrix | Traverse level by level - one level by one. |
| Union Find | Find_parent(x); union(x,y). Union by rank – increase rank when both parents ranks are equal(cover small tree under large). Union by size -> increase everytime we combine 2 node. (when nodes size matter). Path compression – point everyone to super parent anyway we care about super parent only. | 684. Redundant Connection | |
| Topological Sort (DFS) | Perform DFS and create a stack such that all u's comes after v's. Then pop and return it. (cycle check might be need if told) | | |
| Topological Sort(CFG) | Add u to stack after all v. And return stack reversed. | | |
| Khans – Topo Sort(BFS) | Create indegree array and add independent nodes and run bfs; Reduce indegree as we remove from queue and if no dependency anymore – add to queue (creating new roots by removing) | 210. Course Schedule II | In degree array + bfs + create new roots by removing parent. Children's are depends on parent. |
| Dijkstra's shortest path. | 1. Distance b/w src to all nodes 2. uses heap to get min total distance node from src 3. Push the total path distance to the heap. # fetch node with minimum known total distance from original source (distance_from_src,node) | 743. Network Delay Time | Add total path to minheap. Visited set not needed. Because a Min-Heap always pops the smallest cumulative weight first, the very first time a node u is popped from the heap, its popped distance (wt) is guaranteed to be its absolute shortest distance from the source. |
| Prims MST(about nodes rather edges focused like Dijkstra's) | build # fetch edge with minimum weight that connects a visited node to a un visited node (nodes_edge_weight, node) 1. No starting point. Edge weight in heap unlike total_path weight in dijikstra. | 1135. Connecting Cities With Minimum Cost | Add edge weight alone to min heap. Visited set needed.(As when poppe. Add to total weight as we pop |
| Kruskal MST | Sort by edge weights. Connect using DSU. DSU ignore duplicate connection itself, no need visited set | Edges.sort(key = lambda x:x[2]) | |
| Floyd Warshall | All path shortest distance algorthim.(distance matrix) Matrix[u][v] = min(matrix[u][v], matrix[u][k]+matrix[k][v]). Take a node and consider that a intermediate node for all possible combination. | Take a node as intermediate. Try all u->v's K=0 U=0 V=0,1,2,3,4 K=1 U=0 V=0,1,2,3,4 [0,1,2,3,4] when k=1 we try (0,0)(0,1)(0,2)(0,3)(0,4) / (0,4)-> (0,1)+(1,4).(warch bari) | |
| Hierholzer (while dfs+topo stack) | Track visited edge without external data structure and allows duplicate edges. You've hit on the exact reason why Hierholzer's algorithm for finding an Eulerian Path uses a while loop with .pop() instead of a standard for loop. - concurrent modification error. | 332. Reconstruct Itinerary | Use every edge once and add no neighbors node to result one by one. |

Drive link - https://drive.google.com/file/d/1E-DfiKFUJW3F8DJaG7Yb1TXeSY69VgjS/view?usp=sharing

| | |
|---|---|
| DFS Undirected Graph Cycle (GFG) | When a v is already visited and not also a parent - then cycle |
| DFS Directed Graph Cycle(GFG) | Multiple path exist in dag since not free way. So path array using backtracking important. Add to path array before visiting neighbour and remove after completion neighbours. |
| DFS Bipartite | shortest path. |
| Bellman Ford | Works for –ve edges and cycles. Relax edges v-1 times using simple for loop but input should in edge list(u,v,w) format. Maximum acyclic path edges = V-1. Because after visiting all vertices, you cannot add another edge without revisiting a vertex. This is why cycle detection works by seeing already seen node. |
| Find Groups Strongly Connected Componene ts | Mutually reachable nodes in DAG. Sort edges – so we have all groups in order instead of randomly processing it. Reverse the graph so non mutual reachable nodes will be blocked. Mutually reachable nodes will be only be there since it is both way ended. Run dfs with stack order and same like island problem count it. Start DFS from a component that has no outgoing edge in the reversed graph.(independent ones) The stack is not for sorting edges; it is storing DFS finishing times. |
| Bridges | |
| Articulation Point | |
| Min Cut | |
| Max Flow | |

**Observations**

Keeping the value in queue to know the path value `q = deque([(src,1)])`

Traversal/path/level bfs/dfs

If there is cycle then there is valid topological order — Topo sort

We can find cycle with dfs, bfs, dsu, toposort.

| Normal DFS | Hierholzer |
|---|---|
| "When I visit a node, record it." | "When I'm done with all outgoing edges, record it." |

**Removing edge in Hierholzer**

we remove edge because again we can reach the same node and we should visit the already visited edge, may be i visited node a but cannot make it visited i may again visit with different edge so we removing the edge

We remove the edge because we may visit the same node again through a different edge, and we still need to use the remaining edges from that node.

we can also track the visited edges and skip but we have to store the tuple and check it cannot cover double path

This algorithm is Hierholzer's algorithm for finding an Eulerian path.

This is an excellent question. Instead of memorizing the itinerary problem, you should recognize when Hierholzer's algorithm applies.

**The pattern**

Ask yourself:

"Am I trying to use every edge exactly once?"

If the answer is yes, think of Eulerian Path/Circuit → Hierholzer's Algorithm.

Notice the focus is on edges, not nodes.

| Requirement | Algorithm |
|---|---|
| Visit every node | DFS / BFS |
| Visit every edge once | Hierholzer (Eulerian Path) |
| Respect dependencies between nodes | Topological Sort |
| Find minimum distance | Dijkstra / BFS |
| Visit every node exactly once | Hamiltonian Path (backtracking/DP, NP-hard) |

**Another reason**

Normal DFS asks: "Which neighbors haven't I visited?"

Hierholzer asks: "Are there any unused edges left?"

That's why the logic is naturally:

```
while there are unused edges:
    take one edge
```

instead of

```
for every neighbor:
    visit neighbor
```

The biggest conceptual shift is:

- Normal DFS is node-centric ("Have I visited this node?")
- Hierholzer is edge-centric ("Have I already used this edge?")

Once you see that distinction, the while loop feels much more natural.

| Normal DFS | Hierholzer DFS |
|---|---|
| Iterate over neighbors | Consume edges one by one |
| `for nei in graph[node]` | `while adj[node]:` |
| Uses visited set | Removes edges (pop()) |
| Nodes are processed once | Nodes may be revisited many times |
| Edges stay in graph | Edges disappear after use |

## 12. Backtracking

Backtracking – leave it free end and block edge base +ve and –ve cases in recursion so easy to reason about – let it explore and fail.

This pattern—boundary check → character check → mark visited → recurse → restore—is the standard backtracking template for Word Search

**N Queens choosing direction**

Top/Bottom - move rows ; Left/right - move columns

Top/left = -1; Bottom/right = +1

Top left = -1 –1

Bottom left = +1, -1

Left = -1

Always mention top/bottom then right/left because it is row/column respectively.

*(diagrams of N-Queens knight/chess movement omitted — see original PDF)*

## 13. Dynamic Programming

```python
# States – information which will be changing & needed to solve the
base case problem
Def f(states...)
    Min/max/count/Bool
    For all available options:
        CurrentPath = f(next state movement, value)
        Min(mini, currentPath)
    Return Min
```

### Dynamic Programming

min/max/count/bool + all possible next state movement in the give space.

Top down – move to different states (what all options I have in future)

Bottom up – take from different states (what all options I have in past)

| | | |
|---|---|---|
| 1D DP | Array based previous state (Fibonacci style) | 746. Min Cost Climbing Stairs |
| 01 Knapsack | Pick or skip with capacity constraint | 416. Partition Equal Subset Sum |
| Unbounded Knapsack | Unlimited supply + pick(or) skip with capacity constraint | 518. Coin Change II |
| 2D DP | State movement on given direction | 174. Dungeon Game |
| 3D DP | State movement on nested options | 741. Cherry Pickup |
| LIS | Track previous to decide current state movement | 300. Longest Increasing Subsequence |
| String DP | Compare 2 strings(Decision) | 1143. Longest Common Subsequence |
| Partition DP | Try all subarray cuts | 1547. Minimum Cost to Cut a Stick |
| Game DP | Track which player turn or use parity = (j - i - N) % 2 | 877. Stone Game |
| State Machine DP | Multiple nodes like stock buy sell | 309. Best Time to Buy and Sell Stock with Cooldown |
| Tree DP | Post order traversal. Answer of nodes depends on answers of children. | 337. House Robber III. |
| Shapes DP | | 221. Maximal Square |
| Probability DP | Each incoming else continue to Branch & sum up stop prob. | 688. Knight Probability in Chessboard |
| Bit DP | Each state represents subset of elements. | 1986. Minimum Number of Work Sessions to Finish the Tasks |
| Digit DP | Tight flag defines upper bound | 357. Count Numbers with Unique Digits |

**Top Down – this is target -> use all options**

**Bottom Up – this is options what all you can form**

**1D Dynamic Programming**

```python
def function(i): #for each index try
    # available options and pick min
    if i == n-1:
        return 0
    ans = float('inf')
    for jump in range(1, k+1): #available options
        nxt = i + jump
        if nxt < n:
            #current path -> next state movement
            cost = abs(height[i] - height[nxt]) + function(nxt)
            ans = min(ans, cost)
    return ans
return function(0)
```

```python
dp = [float('inf')] * n
dp[0] = 0
for i in range(1, n): #for each index try avaiable options
    for j in range(1, k + 1):
        if i - j >= 0:
            #current path
            cost = dp[i-j] + abs(height[i] - height[i-j])
            dp[i] = min(dp[i], cost) # take min
return dp[n-1]
```

**01 Knapsack – element can be selected only once.**

```python
def isSubsetSum(self, arr, sum):
    n = len(arr)
    def f(i, sum): #target
        if sum == 0:
            return True
        if i == n:
            return False
        canForm = False
        for j in range(i, n): #options avaiable
            if arr[j] <= sum:
                currentPath = f(j+1, sum-arr[j])
                canForm = canForm or currentPath
        return canForm
    return f(0, sum)
```

```python
dp = [[False for _ in range(target+1)] for _ in range(n)]
if nums[0] <= target:
    dp[0][nums[0]] = True

for i in range(1, n): #options
    for tar in range(1, target+1): #target
        pick = False
        if nums[i] <= tar:
            pick = dp[i-1][tar-nums[i]] #i+1
        skip = dp[i-1][tar] #implict skip
        dp[i][tar] = pick or skip
return dp[n-1][target]
```

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        n = len(prices)
        dp = [[-1 for _ in range(2)] for _ in range(n)]
        def util(i, buy): # 4 options we have
            if i >= n:
                return 0
            if dp[i][buy] != -1:
                return dp[i][buy]
            if buy == 1:
                profit = max(
                    0 + util(i+1,1),
                    -prices[i] + util(i+1,0)
                )
            elif buy == 0:
                profit = max(
                    0 + util(i+1, 0),
                    prices[i] + util(i+2, 1)
                )
            dp[i][buy] = profit
            return dp[i][buy]
        return util(0,1)
```

```python
def maxProfit(self, prices: List[int]) -> int:
    n = len(prices)
    if n <= 1:
        return 0
    b, s = 0, 1
    # Using exact size n since we handle boundaries inline
    dp = [[0] * 2 for _ in range(n)]
    for i in range(n): # 4 options we have
        # BUY: Look back 2 days ago (dp[i-2][1]) for the cooldown
        buy = -prices[i] + (dp[i-2][s] if i >= 2 else 0)
        # Default to -inf on day 0 because holding without buying is
        # impossible(invalid option)
        skip_hold = (dp[i-1][b] if i >= 1 else -float('inf'))
        dp[i][b] = max(buy, skip_hold)
        # SELL: Look back 1 day ago to see if we can sell or skip selling
        sell = prices[i] + (dp[i-1][b] if i >= 1 else -float('inf')) #(invalid option)
        skip_sell = (dp[i-1][s] if i >= 1 else 0)
        dp[i][s] = max(sell, skip_sell)
    return dp[n-1][1] # Return the max profit on the last day with no stock held
```

States – information which will be changing and needed to solve the base case problem

Top down – move to different states (what all options I have in future)

Bottom up – take from different states (what all options I have in past)

Direction is different.

1. Always try to form the states using information/data in the problem
2. Think answer for single problem using bottom up approach
3. Think how to divide the problem using top down approach

```
state
  ↓
try every possible action
  ↓
go to next state
  ↓
take best/sum/min/max
  ↓
store in dp
  ↓
Return
```

**An even better abstraction**

Whenever you see a DP problem, ask these four questions:

a. What uniquely identifies a state?
   - i
   - (i, j)
   - (i, j, k)
   - (mask, i)
   - (node, parent)
b. What choices do I have from this state?
c. How do I combine the results of those choices?
   - max
   - min
   - sum
   - or
d. Can two paths reach the same state?
   - If yes → memorize.

Tech dose DP: https://www.youtube.com/watch?v=RElcqtFYTm0&list=PLEJXowNB4kPxBwaXtRO1qFLpCzF75DYrS&index=37

-----------------------------

**DP knowledge**

Target = n

Need -> Different ways = count

Options = climb1 and climb2

Define states(variables) need to solve the base case(smallest problem)

Then move different options and take min/max/count all of all

**Core DP mindset**

1. First ask: "What does dp[i] represent?"
2. Then ask: "What smaller states can produce this state?"

**Top-down (Memoization) - reach base case**

Think recursively:

"To solve this problem, what smaller problems do I need?"

- Write recursion → add memoization.
- Start from the final/big problem → break down.

**Bottom-up (Tabulation) - start from base case**

Think iteratively:

"What are the smallest states, and how do I build bigger states from them?"

- Identify base cases → fill table → reach answer.
- Start from small problems → build up.

**Shortcut:**

Top-down = Problem → smaller problems

Bottom-up = Small problems → final problem

**Core Takeaway**

- Forward ($0 \to N$): Best when standing at step $i$ mandates a decision that pays off in future steps (e.g., jump 1 vs. jump 2, buy/sell stock today vs. tomorrow).
- Backward ($N \to 0$): Best when the final goal $N$ is fixed and you evaluate past options that could have produced that goal (e.g., $N^{\text{th}}$ Fibonacci, Coin Change amount $A$, Knapsack capacity $W$).

your intuition is spot on: when an action at $i$ dictates costs incurred in the immediate future ("pay and jump"), a forward formulation ($0 \to N$) feels completely seamless.

can i say the direction can be choosen from the cost we are going to pay

pay and make jump -> natually forward

get the nth fibonnace -> cost should comes from top to decide for me?

give one counter intitive for backward natually like this pay and make jump -> natually forward

**Forward** – when current value should is common for all next steps
- When current decision can be made + rest can be delegated

**Backward** – when current value dependent of smaller problems.
- When current decision itself need value from previous or smaller problems.

## 14. Advanced Data Structures

Trie – key itself the alphabet and reference is for children and end letter or not.

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.isEnd = False

#insert if not available
def insert(self, word: str) -> None:
    current = self.root
    for c in word:
        if c not in current.children:
            current.children[c] = TrieNode()
        current = current.children[c]
    current.isEnd = True
```

**Rule to remember:** A Trie node represents one character at one specific position in a path, not a globally unique character. You can have many different 't' nodes in a Trie as long as they occur on different paths.

## 15. Maths

### What is Greatest Common Divisor (GCD)?

GCD of two numbers is the largest number that divides both numbers exactly (with no remainder).

```python
print(math.gcd(12, 18))
```

**Euclidean Algorithm to find GCD**

```
gcd(a, b) = gcd(b, a % b)
```

```python
def gcd(a, b):
    while b:
        a, b = b, a % b
    return a
```

**Properties:**

1. Two numbers are coprime if: `gcd(a,b) = 1`

   `gcd(8,15)=1` means No common factor except 1.

**How it is helpful?**

2. Relationship with LCM

   `gcd(a,b) * lcm(a,b) = a*b`
