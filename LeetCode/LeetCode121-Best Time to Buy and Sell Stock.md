
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

注意你不能在买入股票前卖出股票。
```
示例 1:

输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
示例 2:

输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock
```

思路一：[一个框架闯天涯](https://blog.csdn.net/zz_daisy/article/details/91358530)中的思路


```python
class Solution:
    def maxProfit(self, prices):
        if len(prices) < 2: return 0
        maxPro = 0
        minPrice = prices[0]
        for sell in range(len(prices)):
            minPrice = min(minPrice, prices[sell])
            maxPro = max(maxPro, prices[sell] - minPrice)
        return maxPro
```

时间复杂度 O(n)  
空间复杂度 O(1)

思路二： 套用[股票问题的DP框架](https://blog.csdn.net/zz_daisy/article/details/91411704)


```python
def maxProfit(prices):
    k = 1
    if not prices: return 0
    
    mp = [[[0, float('-inf')] for _ in range(k+1)] for _ in range(len(prices))]
    for i in range(len(prices)):
        for kk in range(k, 0, -1):
            mp[i][kk][0] = max(mp[i-1][kk][0], mp[i-1][kk][1] + prices[i])
            mp[i][kk][1] = max(mp[i-1][kk][1], mp[i-1][kk-1][0] - prices[i])
    return mp[len(prices)-1][k][0]
```

思路三：对思路二优化  
  
```
mp[i][1][0] = max(mp[i-1][1][0], mp[i-1][1][1] + prices[i])
mp[i][1][1] = max(mp[i-1][1][1], mp[i-1][0][0] - prices[i])
        = max(mp[i-1][1][1], -prices[i])  # mp[i-1][0][0] = 0
此时，k = 1，对状态转移没有影响，可以去掉，状态转移方程即：
mp[i][0] = max(mp[i-1][0], mp[i-1][1] + prices[i])
mp[i][1] = max(mp[i-1][1], -prices[i])  
并且，初始状态为：
dp[i][0] = 0
dp[i][1] = -prices[i]
```


```python
class Solution:
    def maxProfit(self, prices):
        if len(prices) < 2: return 0
        mp = [[0, -prices[0]] for _ in range(len(prices))]
        for i in range(len(prices)):
            mp[i][0] = max(mp[i-1][0], mp[i-1][1] + prices[i])
            mp[i][1] = max(mp[i-1][1],  -prices[i])
        return mp[len(prices)-1][0]
```

时间和空间复杂度均为 O(n)

思路四：进一步优化  
  
我们可以发现 mp[i][0] 或 mp[i][1] 只和 mp[i-1][0] 或 mp[i-1][1] 有关，所以可以进行压缩状态，只用两个变量来迭代更新即可，这样减少空间复杂度为 O(1)


```python
class Solution:
    def maxProfit(self, prices):
        if len(prices) < 2: return 0
        mp_i_0, mp_i_1 = 0, -prices[0]
        for i in range(len(prices)):
            mp_i_0 = max(mp_i_0, mp_i_1 + prices[i])
            mp_i_1 = max(mp_i_1,  - prices[i])
        return mp_i_0
```
