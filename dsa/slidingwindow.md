# Sliding Window

## Minimum Window Substring

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        m, n = len(s), len(t)
        tcnt = Counter(t)
        scnt = Counter()
        l = r = 0
        mini = 10**9
        matched = 0
        sidx = -1

        while r < m:
            cur = s[r]
            scnt[cur] +=1
            if cur in tcnt and scnt[cur] == tcnt[cur]:
                matched += 1

            while matched == len(tcnt):
                if (r-l+1) < mini:
                    mini = r-l+1
                    sidx = l

                lchar = s[l]
                scnt[lchar] -= 1
                if lchar in tcnt and scnt[lchar] < tcnt[lchar]:
                    matched -= 1
                l += 1

            r += 1

        return s[sidx:sidx+mini] if sidx != -1 else ""
```

## Longest Repeating Character Replacement

```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        counter = Counter()
        maxi = ans = 0
        l = r = 0
        while r < len(s):
            cur = s[r]
            counter[cur] += 1
            maxi = max(maxi, counter[cur])
            # totalwindow - max element
            while (r-l+1) - maxi > k:
                counter[s[l]] -= 1
                l += 1 #push till k voilation
            ans = max(ans, r-l+1)
            r += 1
        return ans
```
