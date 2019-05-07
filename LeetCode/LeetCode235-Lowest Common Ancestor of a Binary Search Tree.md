
给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]  

```
         6
        /  \
       2    8
      / \  / \
     0   4 7  9
        / \
       3   5
```

示例 1:  

输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8  
输出: 6   
解释: 节点 2 和节点 8 的最近公共祖先是 6。  
  
示例 2:  

输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4  
输出: 2  
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。  

说明:  

- 所有节点的值都是唯一的。  
- p、q 为不同节点且均存在于给定的二叉搜索树中。  

  
思路一：  
利用BST的性质，root的左子树的值均小于root.val，右子树的值均大于root.val  
则有，若q.val 和p.val的值均大于root.val, 则p,q肯定在root的右子树，若小于root.val则p,q肯定在root的左子树，否则q,p肯定在root的两侧，即root为p,q的最近公共祖先。  
时间复杂度：O(n)

下面是递归实现：


```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        if not root: return root
        if q.val < root.val > p.val:
            return self.lowestCommonAncestor(root.left, p, q)
        if q.val > root.val < p.val:
            return self.lowestCommonAncestor(root.right, p, q)
        return root
```

迭代版本


```python
class Solution:
    def lowestCommonAncestor(self, root, p, q):
        while root:
            if p.val < root.val > q.val:
                root = root.left
            elif p.val > root.val < q.val:
                root = root.right
            else:
                return root
```

思路二：此方法也适用于二叉树的最近公共祖先  
分别从根节点寻找节点q,p，找节点过程中记录路径，路径即链表。  
然后看从根节点到两个节点的路径何时不相同了,最近公共祖先就为前一个节点  

如p = 5, q = 9  
```
pathp = 6->2->4->5  
pathq = 6->8->9
```
代码，写起来比较麻烦，晓得思想就好了吧~~~~

思路三：此方法同样适用于二叉树的最近公共祖先  
对于root,如果p,q能在其左右子树中找到，则root为其公共最近祖先。  
若在root左子树找不到p或q，则root不可能是其公共祖先  
同样地，root右子树找不到p或q，则root不可能是其公共祖先


```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root or root == p or root == q: return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if left and right:
            return root
        return left or right
```
