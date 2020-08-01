# Split Array Largest Sum

## Problem Illustration
Given an array which consists of non-negative integers and an integer m, you can split the array into m non-empty continuous subarrays. Write an algorithm to minimize the largest sum among these m subarrays.

Solution: **Possible value with binary search**

## Solution Insights
**The min possible value is the largest number in array. And we can also get an intuitive maximum value by equally spliting the array and get the maximum subarray sum as upper bound solution.**

## Solution Implementation
Binary Search + Equal Array Partition

Solution Sample:
```python
class Solution:
    def splitArray(self, nums: List[int], m: int) -> int:
        maxValue, intervalLength, maxInterval = max(nums), len(nums)//m, 0
        for i in range(m):
            offset, value = intervalLength * i, 0
            intervalEnd = offset + intervalLength + (len(nums)%m if i == m - 1 else 0)
            for j in range(offset, intervalEnd):
                value += nums[j]
            maxInterval = max(maxInterval, value)

        def checkPossible(value):
            intervalCount, currentSum = 0, 0
            for i in range(len(nums)):
                if currentSum + nums[i] > value:
                    intervalCount += 1
                    if intervalCount == m:
                        return False
                    currentSum = 0
                currentSum += nums[i]
            return True
        
        start, end = maxValue, maxInterval
        if checkPossible(start):
            return start
        while end - start > 1:
            mid = (start + end)//2
            if checkPossible(mid):
                end = mid
            else:
                start = mid
        return end
```