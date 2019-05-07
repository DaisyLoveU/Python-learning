
给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]  
```
示例 1:

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1  
输出: 3  
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。  
  
示例 2:  

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4  
输出: 5  
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。  
```
思路一：即235的思路二，从root查找p,q，并记录路径。
1. Find path from root to n1 and store it in a vector or array.
2. Find path from root to n2 and store it in another vector or array.
3. Traverse both paths till the values in arrays are same. Return the common element just before the mismatch.  

时间复杂度O(n)  
空间复杂度O(n)



```python
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        def findPath(root, path, node):
            if not root:
                return False
            path.append(root)
            if root == node:
                return True
            if (root.left and findPath(root.left, path, node)) or \
                (root.right and findPath(root.right, path, node)):
                return True
            path.pop()
            return False
        
        path_p, path_q = [], []
        if not findPath(root, path_p, p) or not findPath(root, path_q, q):
            return -1
        i = 0
        while i < len(path_p) and i < len(path_q):
            if path_p[i] != path_q[i]:
                break
            i += 1
        return path_p[i-1]
```

思路二：即235的思路三  
每个节点都需要访问，则时间复杂度O(n)    
使用了递归，其空间复杂度为树的深度，题目给出二叉树，则最坏空间复杂度O(n)


```python
class Solution:
    def lowestCommonAncestor(self, root, p, q):
        if not root or root == q or root == p:
            return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        return root if left and right else left or right
```
