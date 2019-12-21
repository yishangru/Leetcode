# Minimum Height Trees

Solution: **BFS + DFS to find the longest path and select middle node as the root node**

Solution Insights: **the root node in MHT must be the middle node of the longest path in graph**

Proof:
- Case 1: Root node is the node in longest path
For this case, the height for the left subtree and right subtree must be the same as the distance from this root node to the left and right end nodes of the longest path in graph. It is easy to prove with contradiction. **If there are nodes with larger distance to root than the end nodes, the longest path is no longer valid. And the true longest path must contain that node as end node.**

- Case 2: Root node isn't the node in longest path
For this case, since the root node isn't the node in the longest path, **the end nodes of longest path must in left subtree or right subtree, otherwise the root node must a node in the longest path**. Assume the longest path is in left subtree. Due to the tree structure, each node only has one parent, **leading the height of this longest path subtree is the same as the tree constructed using nodes in the longest path as root node**. However, there are some distance between the root node of this subtree and the root node of the whole tree, the height of the whole tree must be greater than the subtree, leading the solution not as minimum height.

## Proof for BFS + DFS for longest path

In order to find longest path in graph, we first prove the availability for BFS and DFS to find the longest path. According to above proof, it is easy to see we can randomly select a node as root node for tree to perform BFS. The leaf node with maximum distance from the root node must be a end node in the longest path.

- Case 1: Root node is the node in longest path
Trivial to prove.

- Case 2: Root node isn't the node in longest path
Same as above, the longest path must in left subtree or right subtree. Assume the longest path is in left subtree. We use contradiction to finish our proof. If the node with maximum distance from root is not the end nodes in longest path, **there is a path from that node to the root and then to the end nodes (select the end node with greater distance to the root node)**. Since the distance from that node to root is greater than the distance from any end node to root, there is a longer path from that node to the other end node in longest path, leading the original longest path invalid. We then prove that the node with largest distance to root must be one of the end nodes in longest path.


## Solution Implementation
From above, we can use BFS and DFS to find the longest path in the graph. For the first BFS, find one of the end nodes in the longest path. The second DFS starts from that found end node and find the path to the other end node (longest distance from that starting end node). After finding the longest path in graph, select middle node of the longest node as solution (for odd, select middle; for even, select both).


Solution Sample (Python - two dfs):
```
import collections
class Solution:
    def findMinHeightTrees(self, n: int, edges: List[List[int]]) -> List[int]:
        if n == 1:
            return [0]
        
        # dfs is better choice, comparing with bfs
        self.graphRecorder = collections.defaultdict(lambda:set())
        # generate graph for transverse
        for edge in edges:
            self.graphRecorder[edge[0]].add(edge[1])
            self.graphRecorder[edge[1]].add(edge[0])
            
        # first dfs for find one end in longest path
        self.visitedNode = set()
        self.longestPath = None
        self.dfs(edges[0][0], list())
        
        longest_path_end = self.longestPath[-1]
        
        # second dfs for find the path to the other end in longest path
        self.visitedNode = set()
        self.longestPath = None
        self.dfs(longest_path_end, list())
        
        return [self.longestPath[len(self.longestPath)//2]] if len(self.longestPath)%2 == 1 else [self.longestPath[len(self.longestPath)//2 - 1], self.longestPath[len(self.longestPath)//2]]
    
    def dfs(self, root, pathHolder):
        self.visitedNode.add(root)
        pathHolder.append(root)
        whetherLeaf = True
        for neighbor in self.graphRecorder[root]:
            if neighbor not in self.visitedNode:
                whetherLeaf = False
                self.dfs(neighbor, pathHolder)
        if whetherLeaf and (self.longestPath is None or len(pathHolder) > len(self.longestPath)):
            self.longestPath = list(pathHolder)
        pathHolder.pop(-1)
```