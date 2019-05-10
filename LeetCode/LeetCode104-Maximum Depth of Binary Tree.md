
给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

说明: 叶子节点是指没有子节点的节点。
```
示例：
给定二叉树 [3,9,20,null,null,15,7]，

    3
   / \
  9  20
    /  \
   15   7
返回它的最大深度 3 。
```  

思路一：递归分治，分别求左子树和右子树的深度


```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solutuin:
    def maxDepth(self, root):
        if not root: return 0
        left = self.maxDepth(root.left) + 1
        right = self.maxDepth(root.right) + 1
        return max(left, right)
```

思路二：使用层次遍历，记录层级


```python
class Solutuin:
    def maxDepth(self, root):
        if not root: return 0
        res = 0
        queue = [root]
        while queue:
            res += 1
            for _ in range(len(queue)):
                node = queue.pop(0)
                if node.left: queue.append(node.left)
                if node.right: queue.append(node.right)
        return res
```

思路三：使用DFS


```python
class Solutuin:
    def maxDepth(self, root):
        if not root: return 0
        self.res = 0
        self.dfs(root, 0)
        return self.res
    
    def dfs(self, node, level):
        if not node: return
        if self.res < level + 1:
            self.res = level + 1
        self.dfs(node.left, level+1)
        self.dfs(node.right, level+1)
```
