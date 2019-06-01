
给定一个整数，编写一个函数来判断它是否是 2 的幂次方。
```
示例 1:

输入: 1
输出: true
解释: 20 = 1
示例 2:

输入: 16
输出: true
解释: 24 = 16
示例 3:

输入: 218
输出: false
```  
思路：2的幂次方数 在二进制都只有首位的1，其余位都是0  
  
使用位运算去掉 低位的1，判断是否为0即可


```python
class Solution(object):
    def isPowerOfTwo(self, n):
        """
        :type n: int
        :rtype: bool
        """
        if n == 0: return False
        
        n &= n-1
        return n==0 
```
