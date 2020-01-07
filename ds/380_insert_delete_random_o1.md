# Insert Delete Random O(1)

Illustration: **Design a data structure with O(1) insert, delete (check present) and random access**

Solution: **Combined dict and list with delete replacement**

Solution Insights: 

Although delete and insert can be directly implemented by set, **there is no ordering for set**. We can’t implement the random-access using set. Thus, a stronger version implementation is required. As for basic data structure in Python, **only list has ordering relation (random access)**, we then consider list-based extension. List already satisfy the requirement of O(1) insert. List is O(n) for average case delete, thus some improvement is required. As for inspiration, List is only O(1) for deleting the end element. **We need a data structure to record the index of corresponding element and then swap it with the end element**. Then, we can update the position recorder and delete the end element with O(1). **This mapping relation naturally leads to dict structure**. We need an extra dict to record the element-index relation and update them when delete happens.

## Solution Implementation
Dict + List
1.  Random access – using random for generating index and returning element in list
2.  Delete – using dict to get the index of deleting element, swapping it with the end element in list, deleting the new ending element and updating the index relation in dict
3.  Insert – using list append the new element and updating index relation in dict

Solution Sample (Python - dict with list):
```
import random
class RandomizedSet:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.elements = list()
        self.indexMapping = dict()
        

    def insert(self, val: int) -> bool:
        """
        Inserts a value to the set. Returns true if the set did not already contain the specified element.
        """
        if val in self.indexMapping.keys():
            return False
        self.elements.append(val)
        self.indexMapping[val] = len(self.elements) - 1
        return True

    def remove(self, val: int) -> bool:
        """
        Removes a value from the set. Returns true if the set contained the specified element.
        """
        if val not in self.indexMapping.keys():
            return False
        temp = self.elements[-1]
        self.elements[-1] = self.elements[self.indexMapping[val]]
        self.elements[self.indexMapping[val]] = temp
        self.indexMapping[temp] = self.indexMapping[val]
        del self.indexMapping[val]
        self.elements.pop(-1)
        return True
        

    def getRandom(self) -> int:
        """
        Get a random element from the set.
        """
        if len(self.elements) > 0:
            return self.elements[random.randint(0, len(self.elements) - 1)]
        return None
```
