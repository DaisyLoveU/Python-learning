
给定一个整数数组 prices，其中第 i 个元素代表了第 i 天的股票价格 ；非负整数 fee 代表了交易股票的手续费用。

你可以无限次地完成交易，但是你每次交易都需要付手续费。如果你已经购买了一个股票，在卖出它之前你就不能再继续购买股票了。

返回获得利润的最大值。
```
示例 1:

输入: prices = [1, 3, 2, 8, 4, 9], fee = 2
输出: 8
解释: 能够达到的最大利润:  
在此处买入 prices[0] = 1
在此处卖出 prices[3] = 8
在此处买入 prices[4] = 4
在此处卖出 prices[5] = 9
总利润: ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
注意:

0 < prices.length <= 50000.
0 < prices[i] < 50000.
0 <= fee < 50000.

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路一：[一个框架闯天涯](https://blog.csdn.net/zz_daisy/article/details/91358530)中的思路


```python
def maxProfit(prices, fee):
    
    def dp(buy):
        if buy >= len(prices): return 0  
        if buy in cache: return cache[buy]
        
        maxPro = 0
        minPri = prices[buy]
        for sell in range(buy+1, len(prices)):
            minPri = min(minPri, prices[sell])
            maxPro = max(maxPro, dp(sell + 2) + prices[sell] - minPri - fee)  
        cache[buy] = maxPro
        return maxPro
    
    cache = {}
    return dp(0)

```

思路二： 套用[股票问题的DP框架](https://blog.csdn.net/zz_daisy/article/details/91411704)

此时 K = infinity, 每次交易需要缴纳手续费，只需在买入或卖出的时候减去手续费即可. 套用模版


```python
class Solution:
    def maxProfit(prices, fee) -> int:
        mp = [[0, float('-inf')] for _ in range(len(prices))]
        for i in range(len(prices)):
            mp[i][0] = max(mp[i-1][0], mp[i-1][1] + prices[i])
            mp[i][1] = max(mp[i-1][1], mp[i-1][0] - prices[i] - fee)
        return mp[len(prices)-1][0] 

```

思路三：优化思路二


```python
class Solution:
    def maxProfit(self, prices, fee):
        if len(prices) < 2: return 0
        mp_i_0, mp_i_1 = 0, -prices[0]
        for i in range(len(prices)):
            tmp = mp_i_0
            mp_i_0 = max(mp_i_0, mp_i_1 + prices[i])
            mp_i_1 = max(mp_i_1, tmp - prices[i] - fee)
        return mp_i_0
```
