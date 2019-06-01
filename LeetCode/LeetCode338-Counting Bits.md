
思路一：直接使用 hammingWeight 函数  O(n*num of 1)


```python
class Solution(object):
    def countBits(self, num):
        """
        :type num: int
        :rtype: List[int]
        """
        def hammingWeight(n):
            cnt = 0
            while n != 0:
                n &= n -1
                cnt += 1
            return cnt

        res = []
        for i in range(num+1):
            res.append(hammingWeight(i))
        return res
```

思路二：使用位运算  

`i >> 1`去掉 i 的最低位；因`(i >> 1) < i` ，故`result[i >> 1]`已计算，因此i 中 1 的个数为`i >> 1`中 1 的个数加最后一位 1 的个数，  
即为`result[i >> 1] + (i & 1)`


```python
class Solution:
    def countBits(self, num):
        res = [0]*(num+1)
        for i in range(1, num+1):
            res[i] = res[i>>1] + i&1
        return res
```

思路三：使用位运算  

已知 `i & (i-1) < i` ，  
```res[0] = 0 
  res[1] = res[0] + 1 
  res[10] = res[01] + 1
  res[11] = res[10] + 1
  ...
```  

`i & (i - 1)`去掉 i 最右边的一个1；因 `i & (i - 1）< i`，故`result[i & (i - 1)]`已计算，所以i中1的个数为`result[i & (i - 1)] + 1`  
  
时间复杂度 O(n)


```python
class Solution:
    def countBits(self, num):
        res = [0]*(num+1)
        for i in range(1, num+1):
            res[i] = res[i&(i-1)] + 1
        return res
```
