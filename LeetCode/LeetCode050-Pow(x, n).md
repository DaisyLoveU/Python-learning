
实现 pow(x, n) ，即计算 x 的 n 次幂函数。  
  
说明:

-100.0 < x < 100.0  
n 是 32 位有符号整数，其数值范围是 [−2^31, 2^31 − 1] 。  

示例 1:  

输入: 2.00000, 10  
输出: 1024.00000  
  
示例 2:

输入: 2.10000, 3  
输出: 9.26100  
  
示例 3:

输入: 2.00000, -2  
输出: 0.25000  
解释: 2^(-2) = 1/2^2 = 1/4 = 0.25  

  
  
思路一：  
调用库函数


```python
class Solution:
    def myPow(self, x, n):
        from math import pow
        return pow(x,n)
```

思路二：  
分治。x^n = x^(n/2) * x^(n/2)


```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if n == 0: return 1
        if n > 0:
            y = self.myPow(x, n//2)
            res = x * y * y if n % 2 else y * y
            return res
        else:
            return 1/self.myPow(x, -n)
```

思路三：  
使用折半计算，每次把n缩小一半，这样n最终会缩小到0，任何数的0次方都为1，这时候我们再往回乘，如果此时n是偶数，直接把上次递归得到的值算个平方返回即可，如果是奇数，则还需要乘上个x的值。还有一点需要引起我们的注意的是n有可能为负数，对于n是负数的情况，令x=1/x, n= -n。  
3^9 = 3 * 3^8  
3^8 = (3^2)^4 = 9^4 = (9^2)^2 = 81^2


```python
class Solution:
    import pysnooper
    @pysnooper.snoop()
    def myPow(self, x: float, n: int) -> float:
        if n < 0:
            x = 1/x
            n = -n
        res = 1
        while n:
            if n & 1:  # n % 2
                res *= x
            x *= x
            n >>= 1  # n //= 2
        return res
```


```python
s = Solution
s.myPow(2, 2, 10)
```

    Starting var:.. n = 10
    Starting var:.. self = 2
    Starting var:.. x = 2
    19:50:38.226233 call         4     def myPow(self, x: float, n: int) -> float:
    19:50:38.226233 line         5         if n < 0:
    19:50:38.226233 line         8         res = 1
    New var:....... res = 1
    19:50:38.226233 line         9         while n:
    19:50:38.226233 line        10             if n & 1:  # n % 2
    19:50:38.227233 line        12             x *= x
    Modified var:.. x = 4
    19:50:38.227233 line        13             n >>= 1  # n //= 2
    Modified var:.. n = 5
    19:50:38.227233 line         9         while n:
    19:50:38.227233 line        10             if n & 1:  # n % 2
    19:50:38.227233 line        11                 res *= x
    Modified var:.. res = 4
    19:50:38.227233 line        12             x *= x
    Modified var:.. x = 16
    19:50:38.227233 line        13             n >>= 1  # n //= 2
    Modified var:.. n = 2
    19:50:38.227233 line         9         while n:
    19:50:38.227233 line        10             if n & 1:  # n % 2
    19:50:38.227233 line        12             x *= x
    Modified var:.. x = 256
    19:50:38.227233 line        13             n >>= 1  # n //= 2
    Modified var:.. n = 1
    19:50:38.227233 line         9         while n:
    19:50:38.227233 line        10             if n & 1:  # n % 2
    19:50:38.228232 line        11                 res *= x
    Modified var:.. res = 1024
    19:50:38.228232 line        12             x *= x
    Modified var:.. x = 65536
    19:50:38.228232 line        13             n >>= 1  # n //= 2
    Modified var:.. n = 0
    19:50:38.228232 line         9         while n:
    19:50:38.228232 line        14         return res
    19:50:38.228232 return      14         return res
    Return value:.. 1024
    




    1024


