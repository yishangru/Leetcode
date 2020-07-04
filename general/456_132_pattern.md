# 132 Pattern

## Problem Illustration
Given a sequence of n integers a1, a2, ..., an, a 132 pattern is a subsequence ai, aj, ak such that i < j < k and ai < ak < aj. Design an algorithm that takes a list of n numbers as input and checks whether there is a 132 pattern in the list.

Solution: **Max Min Stack**

## Solution Insights
**For each number, we want to check whether it can be the k to form 132 pattern. Thus, we want to find a previous j and i, with i small enough, and j big enough.**

Proof:
Both the maxStack and minStack are with decreasing trend. As for time complexity, since **each element in list will at most insert and pop out the max stack at once**, it is O(n). 

## Solution Implementation
We maintain two stacks. One is to record the min element when reaching certain index. One is to record the previous big element (bigger than previous min) as max stack. We can perform following optimization:
1. Only record the decreasing sequence for the max stack: since the min keeps decreasing, once we found a element is greater or equal to tail element in stack, we can possible get a bigger j with and a smaller i. Thus, we can just remove that smaller tail element in repeated manner and use the new one as tail.
2. Update max stack before checking possible 132 pattern: if we come to a element is greater than current tail, we can't find a 132 pattern for that tail and we need to remove that tail.

Solution Sample:
```python
class Solution:
    def find132pattern(self, nums: List[int]) -> bool:
        if len(nums) < 3:
            return False
        minStack = [nums[0]]
        maxStack = []
        for i in range(1, len(nums)):
            while len(maxStack) > 0 and nums[i] >= maxStack[-1][0]: # remove previous impossible element
                maxStack.pop(-1)
            if maxStack and nums[i] < maxStack[-1][0] and nums[i] > maxStack[-1][1]:
                return True
            if nums[i] > minStack[-1]:
                maxStack.append((nums[i], minStack[-1]))
            if nums[i] < minStack[-1]:
                minStack.append(nums[i])
        return False
```