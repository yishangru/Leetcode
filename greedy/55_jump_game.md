# Jump Game

## Problem Illustration
Given an array of non-negative integers, you are initially positioned at the first index of the array. Each element in the array represents your maximum jump length at that position. Determine if you are able to reach the last index.

Solution: **Greedy with record for maximum reachable index**

## Solution Insights
**Greedy for maximum reachable index**

Proof:
For each index, if it is reachable, there must be a previous reachable index with step that can reach that index. Thus, we need to reach the maximum reachable index after reaching a index, updating the maximum. If a index is greater than current maximum, the index can't be reached from previous index and there is no way we can proceed to following index.

## Solution Implementation
Greedy search


Solution Sample:
```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        currentReachMax = 0
        for i, ele in enumerate(nums):
            if i > currentReachMax:
                return False
            currentReachMax = max(currentReachMax, i + ele)
            if currentReachMax >= len(nums) - 1:
                return True
```