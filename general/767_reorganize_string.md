# Reorganize String

Illustration: **Given a string S, check if the letters can be rearranged so that two characters that are adjacent to each other are not the same.**

Solution: **Greedy Generation**

## Solution Insights 
Place the high frequency char in the next position if legal.

Proof:

Assume this string is valid to generate such sequence, that no same character is adjacent to each other. Start from the end of string, we transverse pair of string from back. The last two char must be different.

As for next char, if it is already in two char, it must be the same as the last char (must be different), if it is not in two char, we can just add this char to our record. We find the first next char that is already present in our record. At this time, all others char-count are 1, while this char count is 2. Assume we proceed to a step that the current count is not the same as our intuition algorithm: the current char-count is not the maximum of recorded char count.

We can start the replace route as following to make the generation route fits our intuition algorithm.

- Case 1: The next char is the same as the current maximum char-count.
Case 1.1: The current char-count is now the same as maximum. Our intuition algorithm still holds.

Case 1.2: The current char-count is smaller than maximum. We should now take the next into consideration. If the current char-count is the second maximum, our intuition algorithm still holds. If not, we first find the last position that the second maximum just exceeds current char-count. We swap the current char-count with that position (all generation property still holds)

- Case 2: The next char is not the same as the current maximum char-count.
Case 2.1: The current char-count is now the same as maximum. Our intuition algorithm still holds.

Case 2.2: The current char-count is smaller than maximum. It is trivival for a new char, since we can easily find a valid position to place that char without affecting the original max char-count sequence. If the char is not new, we find the last position of current maximum char-count just exceed the current char-count. We can insert the current char-count ahead of the position to generate a valid sequence (Case 2.1).

## Solution Implementation
Implicit string generation and reduction with K update.

Solution Sample (Python):
```python
import collections
import heapq
class Solution:
    def reorganizeString(self, S: str) -> str:
            recorder = collections.defaultdict(int)
        for s in S:
            recorder[s] += 1
        heap = list()
        for s in recorder.keys():
            heap.append((-1 * recorder[s], s))
        heapq.heapify(heap)
        generateString = ""
        for i in range(len(S)):
            target = heapq.heappop(heap)
            if len(generateString) > 0 and generateString[-1] == target[1]:
                if len(heap) == 0:
                    return ""
                else:
                    newTarget = heapq.heappop(heap)
                    generateString += newTarget[1]
                    if newTarget[0] < -1:
                        heapq.heappush(heap, (newTarget[0] + 1, newTarget[1]))
                    heapq.heappush(heap, target)
            else:
                generateString += target[1]
                if target[0] < -1:
                    heapq.heappush(heap, (target[0] + 1, target[1]))
        return generateString
```