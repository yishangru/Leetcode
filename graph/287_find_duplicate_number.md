# Find Duplicate Number

## Problem Illustration
Given an array nums containing n + 1 integers where each integer is between 1 and n (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one. **A number can be duplicated multiple times.**

Solution: **Extension for DFS loop detection - Node Mapping**

## Solution Insights
**Loop detection and loop entry point proof**

Proof:
- DFS Based Loop Detection
The classic 1-2 step dfs loop detection is easy to prove. Since the loop length is fixed, and the distance before entering the loop is fixed. We use contradiction to prove. Assume there is no meeting point. Then, for a given loop length **L** and distance before entering loop **D**, we can't find a step number **t** that satisfies **(2t - D) mod L == (t - D) mod L**. However, we can easily find **t mod L == 0** to meet the condition and there is contradiction. Then we focus on the condition of duplicating number to see whether multiple number (> 2) can affect the correctness. **Since we use the same procedure for 1 step and 2 step, multiple duplication won't affect the correctness.**

- Loop Entry Point Prove
We use DFS to detect the loop and the left problem is the duplication number detection. It is easy for us to see that the duplication number is exactly the entry point of loop (there is multiple path in the graph linked to that entry node - duplication). Then we need to detect the entry point. From above proof, we can know that the step number **t mod L == 0**. Thus, the sum of distance from the loop entry point to the meeting point **Y** and **D** is **L**. Then, if we start a new DFS from starting point and proceed both pointers (1 step pointer from loop detection and the new pointer) **D** steps, these two pointers can meet with each other at the entry point. In another word, the next meeting point would be the entry point of the loop. 

![Program Screenshot](/graph/fig/287.PNG)

## Solution Implementation
We first perform node mapping. Each index is mapped to corresponding node and the element at that index is the reference pointing to next linked node. We first detect the meeting point in the loop and then based on the meeting proof given above, we can again loop the two pointer to find the entry point of loop, which is both the meething point and the duplicate number (multiple path to enter the loop).


Solution Sample:
```
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        # we need a start point whose has no inbound reference, since all node value > 0, value at index is the reference to next node
        pointer1, pointer2 = nums[0], nums[nums[0]]
        while not pointer1 == pointer2:
            pointer1 = nums[pointer1]
            pointer2 = nums[nums[pointer2]]
        pointer3 = 0
        while not pointer3 == pointer1:
            pointer1 = nums[pointer1]
            pointer3 = nums[pointer3]
        return pointer1
```