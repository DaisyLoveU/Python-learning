
实现 int sqrt(int x) 函数。

计算并返回 x 的平方根，其中 x 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。
```
示例 1:

输入: 4
输出: 2
示例 2:

输入: 8
输出: 2
说明: 8 的平方根是 2.82842..., 
     由于返回类型是整数，小数部分将被舍去。```

思路一：暴力循环


```python
class Solution:
    def mySqrt(self, x):
        i = 0
        while i * i <= x:
            if i * i <= x and (i + 1) * (i + 1) > x:
                return i
            i += 1
```

思路二：二分


```python
class Solution:
    def mySqrt(self, x: int) -> int:
        if x == 0 or x == 1: return x
        
        l,r = 1,x
        while l <= r:
            mid = (r+l)//2
            if mid <= x//mid and mid+1 > x//(mid+1):
#                 考虑 mid^2可能超出int界限 使用x//mid更安全
                return mid
            elif mid > x//mid:
                r = mid - 1
            else:
                l = mid + 1
```

思路三：牛顿法求开方

1,求方程```f(x)=0```的根即求曲线```y=f(x)与y=0```的交点的横坐标.  
  
2,牛顿法:也就是从估计点```x0```出发,以```y=f(x0)+f'(x0)(x-x0)```作为对```y=f(x)```的估计,求得根x1.其中```x1=x0-f(x0)/f'(x0)```  
依次迭代.  
  
3,这里就是对曲线```y=f(x)```,使用经过```(x0,f(x0))```点的切线,进行近似f(x).显然该切线的斜率等于曲线的斜率```k=f'(x0)```,那么该切线的方程为```y=f'(x0)(x-x0)+f(x0)```.(这里是牛顿法的核心,也就是使用切线对曲线进行近似)  
  
4,对于求开方也就是求```x^2=a```的解, 这里```f(x)=x^2-a, f'(x)=2x```.所以利用上式:以```y=2x0(x-x0)+x0^2```,则其根为```x1=x0-(x0^2-a)/2x0=(x0+a/x0)/2```  
  
5,例子x^2=2.  
x0=2,  
x1=(2+0.5)/2=1.4998  
x2=(x1+1/x1)/2=1.4167  
x3=1.4142
...



```python
class Solution:
    def mySqrt(self, x):
        t = x
        while t ** 2 > x:
            t = (t + x//t) // 2
        return t
```

附上有精度的二分求开方代码


```python
def fun(x, f=1e-6):
    if x == 0 or x == 1: return x
    l,r = 0,x
    while l <= r:
        mid = (l+r)/2
        if abs(mid**2 - x) < f:
            return mid
        elif mid ** 2 > x:
            r = mid
        else:
            l = mid
```


```python
fun(2)
```




    1.4142136573791504




```python
import math
math.sqrt(2)
```




    1.4142135623730951


