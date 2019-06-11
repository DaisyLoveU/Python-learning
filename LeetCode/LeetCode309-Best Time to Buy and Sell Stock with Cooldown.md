
给定一个整数数组，其中第 i 个元素代表了第 i 天的股票价格 。​

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。
```
示例:

输入: [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路一：[一个框架闯天涯](https://blog.csdn.net/zz_daisy/article/details/91358530)中的思路


```python
def maxProfit(prices):
    
    def dp(buy):
        if buy >= len(prices): return 0  # 边界条件
        if buy in cache: return cache[buy]
        
        maxPro = 0
        minPri = prices[buy]
        for sell in range(buy+1, len(prices)):
            minPri = min(minPri, prices[sell])
            maxPro = max(maxPro, dp(sell + 2) + prices[sell] - minPri)  # 修改交易后的买入时间
        cache[buy] = maxPro
        return maxPro
    
    cache = {}
    return dp(0)

```

思路二： 套用[股票问题的DP框架](https://blog.csdn.net/zz_daisy/article/details/91411704)  
  
此时 K = infinity, 冷冻期含义是 如果我们在第 i-1 天卖出就不能在第 i 天购买股票。因此第 i 天买入的时候，状态应该是第 i-2 天转换来的，代码如下


```python
def maxProfit(prices):
    if len(prices) < 2: return 0

    mp = [[0, float('-inf')] for _ in range(len(prices))]
    for i in range(len(prices)):
        mp[i][0] = max(mp[i-1][0], mp[i-1][1] + prices[i])
        mp[i][1] = max(mp[i-1][1], mp[i-2][0] - prices[i])
    return mp[len(prices)-1][0]

```

思路三：优化思路二  
  
状态压缩，增加一个变量 mp_pre_0 来表示 mp[i-2][0]


```python
class Solution:
    def maxProfit(self, prices):
        if len(prices) < 2: return 0
        mp_i_0, mp_i_1, mp_pre_0 = 0, -prices[0], 0
        for i in range(len(prices)):
            tmp = mp_i_0
            mp_i_0 = max(mp_i_0, mp_i_1 + prices[i])
            mp_i_1 = max(mp_i_1, mp_pre_0 - prices[i])
            mp_pre_0 = tmp
        return mp_i_0
```

时间复杂度 O(n)  
空间复杂度 O(1)
