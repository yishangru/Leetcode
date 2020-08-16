# Longest Repeating Character Replacement

## Problem Illustration
Given a string s that consists of only uppercase English letters, you can perform at most k operations on that string (change an character to any other). Find the length of the longest sub-string containing all repeating letters you can get after performing the above operations.

Solution: **Possible value with binary search**

## Solution Insights
**After we perfrom the possible operations, we can get an interval with same character. Then, the problem become to find the longest interval that we can change all characters within that interval to be the same character. Another insight is that we need to get the current max-frequeny character in that interval to see whether we can change the other to that character. There is an optimization for this problem to prevent the search for update of max-frequency. Once we find a possible solution (maxf + k == interval), we can only find better solutions when we have a new interval (a character showing up for a lot times), which is a possible increasing stack.**

## Solution Implementation
Two pointer solution

Solution Sample:
```python
import collections
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        length, maxf = 0, 0
        recorder = collections.defaultdict(int)
        for i in range(len(s)):
            recorder[s[i]] += 1
            maxf = max(maxf, recorder[s[i]])
            # same as the increasing stack, we can only find better solution when maxf update
            if maxf + k > length:
                length += 1
            else: # we need to update recorder
                recorder[s[i - length]] -= 1
        return length
```