# Stone Game II
Illustration: **Find optimal solution for Game (Max-min problem)**

Solution: DP with state recorder, i.e. the max profit under certain conditions

## Solution Insights
**Max-Min: first step max, next min. Find max given a series results.**

## Solution Implementation
Record the state for each step - a state refers as (m, current Position) and the corresponding max profits with given **(m, current position)** 

Solution Sample (Python - DP):
```
class Solution:
    def stoneGameII(self, piles: List[int]) -> int:
        # minmax problem, search problem
        runningSum = [0]
        for i, pile in enumerate(piles):
            runningSum.append(pile if i == 0 else (runningSum[-1] + pile))
        self.recorder = collections.defaultdict(dict)
        return self.dp(1, 0, piles, runningSum)
    
    def dp(self, m, pointer, piles, runningSum):
        if m not in self.recorder[pointer].keys():
            if pointer + 2*m >= len(piles):
                self.recorder[pointer][m] = runningSum[-1] - runningSum[pointer]
            else:
                currentMin = float('inf')
                for i in range(1, 2*m + 1):
                    currentMin = min(currentMin, self.dp(max(m, i), pointer + i, piles, runningSum))
                self.recorder[pointer][m] = runningSum[-1] - runningSum[pointer] - currentMin
        return self.recorder[pointer][m]
```