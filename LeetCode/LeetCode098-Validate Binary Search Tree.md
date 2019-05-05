
给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

节点的左子树只包含小于当前节点的数。  
节点的右子树只包含大于当前节点的数。  
所有左子树和右子树自身必须也是二叉搜索树。    
```
示例 1:

输入:  
    2  
   / \  
  1   3  
输出: true  
示例 2:  

输入:  
    5  
   / \  
  1   4  
     / \  
    3   6  
输出: false  
解释: 输入为: [5,1,4,null,null,3,6]。  
     根节点的值为 5 ，但是其右子节点值为 4 。
```  
思路一：
```
利用BST的性质，中序遍历后即得到升序数组且BST中不包含相同的元素。  
时间复杂度O(n)  
空间复杂度O(n)
```


```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def isValidBST(self, root):
        def inorder(root):
            if not root: return []
            return inorder(root.left) + [root.val] + inorder(root.right)
        res = inorder(root)
        return res == list(sorted(set(res)))
```

思路二：  
```
优化了思路一的空间复杂度
递归地从root开始，  
判断root.left是否满足 mini<=root.left.val<=root.val  
判断root.right是否满足 root.val<=root.right.val<=maxi  
遍历所有节点，当所有节点均满足以上条件时，return True  
时间复杂度O(n)  
空间复杂度O(1)
```


```python
class Solution:
    def isValidBST(self, root):
        def helper(root, mini, maxi):
            if not root: return True
            if root.val >= maxi or root.val <= mini: return False  # BST不存在相等的元素，因此这里是大于等于和小于等于
            return helper(root.left, mini, root.val) and helper(root.right, root.val, maxi)
        
        maxi = sys.maxsize  # 选了Python的最大值作为初始值
        mini = -maxi
        return helper(root,mini,maxi)
```
