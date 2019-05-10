
给定一个二叉树，返回其按层次遍历的节点值。 （即逐层地，从左到右访问所有节点）。

```
例如:
给定二叉树: [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [9,20],
  [15,7]
]
```  
思路一： 层次遍历  
从根节点开始，一层 一层的处理节点，  
使用双端队列存放每层的节点，visit过一个节点就弹出该节点。  
使用了for loop遍历一层的节点，后续的插入下一层节点不会影响到当前层的遍历。


```python
class Solution:
    def levelOrder(self, root):
        if not root: return []
        res = []
        queue = [root]
        while queue:
            current_level = []
            for _ in range(len(queue)):
                node = queue.pop(0)
                current_level.append(node.val)
                if node.left: queue.append(node.left)
                if node.right: queue.append(node.right)
            res += current_level,
        return res
```

思路二：深度优先遍历，记录深度,


```python
class Solution:
    def levelOrder(self, root):
        if not root: return []
        self.res = []
        dfs(root, 0)
        return self.res
    
    def dfs(self, node, level):
        if not node: return
        if len(self.res) < level+1:
            self.res += [],
        self.res[level].append(node.val)
        
        self.dfs(node.left, level+1)
        self.dfs(node.right, level+1)
```
