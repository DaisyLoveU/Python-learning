
给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

说明: 叶子节点是指没有子节点的节点。
```
示例:

给定二叉树 [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回它的最小深度  2.
```
思路一：  递归分治  

递归退出条件，当前节点为空  
当前节点没有左孩子时，深度等于右孩子的深度  
当前节点没有有孩子时，深度等于左孩子的深度  
当前节点没有左右孩子时，为叶节点。


```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def minDepth(self, root):
        if not root: return 0
        
        if not root.left: return 1 + self.minDepth(root.right)
        if not root.right: return 1 + self.minDepth(root.left)
        
        left = self.minDepth(root.left)
        right = self.minDepth(root.right)
        
        return min(left, right)
```

思路二：BFS，在第i层访问到第一个叶节点时，i为最小深度


```python
class Solution:
    def minDepth(self, root):
        if not root: return 0
        res = 0
        queue = [root]
        while queue:
            res += 1
            for _ in range(len(queue)):
                node = queue.pop(0)
                if not node.left and not node.right:
                    return res
                if node.left: queue.append(node.left)
                if node.right: queue.append(node.right)
```

思路三： DFS,记录叶节点的深度，res初始化为最大数，然后取最小的深度


```python
class Solution:
    def minDepth(self, root):
        if not root: return 0
        self.res = sys.maxsize
        self.dfs(root, 0)
        return self.res
    
    def dfs(self, node, level):
        if not node: return
        if not node.left and not node.right:
            if self.res > level + 1:
                self.res = level + 1
            return
        self.dfs(node.left, level+1)
        self.dfs(node.right, level+1)
```
