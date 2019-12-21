##Minimum Height Trees

Solution Insights: **the root node in MHT must the middle node of the longest path in graph**

Proof:
- Case 1: Root node is the node in longest path
For this case, the height for the left subtree and right subtree must be the same as the distance from this root node to the left and right end nodes of the longest path in graph. It is easy to prove with contradiction. **If there are nodes with larger distance to root than the end nodes, the longest path is no longer valid. And the true longest path must contain that node as end node.**

- Case 2: Root node isn't the node in longest path
For this case, since the root node isn't the node in the longest path, **the end nodes of longest path must both in left subtree or right subtree, otherwise the root node must a node in the longest path**. Assume the longest path is in left subtree. Due to the tree structure, each node only has one parent, **leading the height of this longest path subtree is the same as the tree constructed using nodes in the longest path as root node**. However, there are some distance between the root node of this subtree and the root node of the whole tree, the height of the whole tree must be greater than the subtree, leading the solution not as minimun height.

Solution:
BFS to find the longest path and select middle node as the root node.

```


```