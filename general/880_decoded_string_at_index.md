# Ugly Number II

Illustration: **Reture K-th letter in final string with decoding rules**

For given string:
1. if the character is letter, add this character to final string
2. if the character d is digit, repeat current final string int(d) - 1 times

Solution: **Iteration decode**

## Solution Insights 
If merely record current string and repeat for times, memory is not enough. Thus, we have to find a smart way to represent current string. It is much more efficient to record the current string size to simply string repeats. It is little hard to tell the latent logic and it is easier to say with code. 

## Solution Implementation
Implicit string generation and reduction with K update.

Solution Sample (Python):
```python
class Solution:
    def decodeAtIndex(self, S: str, K: int) -> str:
        stringLength = 0
        for i, s in enumerate(S):
            if s.isdigit():
                stringLength *= int(s)
            else:
                stringLength += 1
            
            if stringLength >= K:
                for j in range(i, -1, -1):
                    K %= stringLength
                    if S[j].isdigit():
                        stringLength /= int(S[j])
                    else:
                        if K == 0:
                            return S[j]
                        stringLength -= 1
        return ""
```