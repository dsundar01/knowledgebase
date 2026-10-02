# Dynamic Programming

## Palindromic Substrings

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        N = len(s)
        dp = [[False] * N for _ in range(N)]

        count = 0
        for i in range(N-1, -1, -1):
            for j in range(i, N):
                leng = j-i+1
                # compare i with all j not all substring b/w i and j
                if s[i] == s[j]:
                    if leng <= 2 or dp[i+1][j-1]:
                        dp[i][j] = True
                        count += 1
        return count
```

## Interleaving String

```python
class Solution:
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        m, n, o =  len(s1), len(s2), len(s3)

        @cache
        def f(i, j, k):
            if k == o:
                return i == m and j == n
            op1 = False
            if i < m and s1[i] == s3[k]:
                op1 = f(i+1, j, k+1)
            op2 = False
            if j < n and s2[j] == s3[k]:
                op2 = f(i, j+1, k+1)
            return op1 or op2
        return f(0, 0, 0)
```

## 312. Burst Balloons

```python
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        nums = [1] + nums + [1]
        n = len(nums)
        lookup = {}

        # maximum profit in the partition (i,j)
        def f(i, j):
            if i > j:
                # maximum profit in invalid partition = 0
                return 0
            if (i, j) in lookup:
                return lookup[(i,j)]
            best = -10**9
            for k in range(i, j+1):
                # burst out of partition; Xi--k---jX
                current = nums[i-1] * nums[k] * nums[j+1]
                left = f(i, k-1)
                right = f(k+1, j)
                best =max(best, current+left+right)
            lookup[(i,j)] = best
            return best

        return f(1, n-2) #burst all valid ballons
```

## Regular Expression Matching

```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        m, n = len(s), len(p)

        @cache
        def f(i,  j):

            if j == n:
                return i == m
            # 1st atom match
            firstmatch = i < m and p[j] in { s[i], '.'}
            if j+1 < n and p[j+1] == '*':
                noocr = f(i, j+2) #ignore the atom
                reuse = firstmatch and f(i+1, j)
                return noocr or reuse
            # regular flow
            return firstmatch and f(i+1, j+1)

        return f(0,0)

```

## Reverse Linked List

```python

```

## Reverse Linked List

```python

```

## Reverse Linked List

```python

```
