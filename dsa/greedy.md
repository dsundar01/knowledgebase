# Greedy

## Jump Game

```python
class Solution:
    def canJump(self, nums: list[int]) -> bool:
        N = len(nums)
        cur = nums[0]

        for i, v in enumerate(nums):
            if i == 0:continue

            #no enough points to reach here
            if cur == 0:
                return False
            cur -= 1
            cur = max(cur, v)
        return True
```

## Jump Game II

```python
class Solution:
    def jump(self, nums: list[int]) -> int:
        N = len(nums)
        if N == 1:return 0
        maxi = current = nums[0]
        # since N > 1
        jumps = 1
        for i in range(1, N-1):
            # reduce the current and maxi
            current -= 1
            maxi -= 1
            # refresh the max
            maxi = max(maxi, nums[i])
            if current == 0:
                current = maxi
                jumps += 1
        return jumps
```

## Gas Station

```python
class Solution:
    def canCompleteCircuit(self, gas: list[int], cost: list[int]) -> int:
        if sum(cost) > sum(gas):
            return -1

        idx = 0
        totalgas = 0
        for i, v in enumerate(gas):
            #collect
            totalgas += gas[i]
            #pay
            totalgas-=cost[i]
            if totalgas < 0:
                idx = i + 1 #next can be answer
                totalgas = 0
        return idx
```

## Hand of Straights

```python
class Solution:
    def isNStraightHand(self, hand: list[int], groupSize: int) -> bool:
        counter = Counter(hand)
        hand.sort()

        for h in hand:
            if counter[h] > 0:
                # current is available in hand
                counter[h] -= 1
                size = 1
                # search next consective element
                while counter[h+size] > 0 and size < groupSize:
                    counter[h+size] -= 1
                    size += 1

                if size < groupSize:
                    return False
        return True


```

## Merge Triplets to Form Target Triplet

```python
class Solution:
    def mergeTriplets(self, triplets: list[list[int]], target: list[int]) -> bool:
        found = set()
        for t in triplets:
            for i, v in enumerate(t):
                #invalid triplet
                if t[0] > target[0] or t[1] > target[1] or t[2] > target[2]:
                    continue
                # index match with direct
                if t[i] == target[i]:
                    found.add(i)
        return len(found) == 3
```

## 763. Partition Labels

```python
class Solution:
    def partitionLabels(self, s: str) -> list[int]:
        lookup = defaultdict(int)

        #prestore the index since we cannot predict future
        for i, v in enumerate(s):
            lookup[v] = i

        res = []
        maxIndex = 0
        size = 0
        for i, v in enumerate(s):
            #max index of current char vs already in thr group char
            maxIndex = max(lookup[v], maxIndex)
            size += 1
            if maxIndex == i:
                res.append(size)
                size = 0
        return res
```

## Valid Parenthesis String

```python
class Solution:
    def checkValidString(self, s: str) -> bool:
        open, star = [], []
        for i, si in enumerate(s):
            if si == '(':
                open.append(i)
            elif si == '*':
                star.append(i)
            else:
                # no open and no star to match
                if not open and not star:
                    return False
                # 1st use open if not available use star
                if open:
                    open.pop()
                elif star:
                    star.pop()
        while open and star:
            # all open should be on left of start
            if open.pop() > star.pop():
                return False
        return not open
```
