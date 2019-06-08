
给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 k 笔交易。

注意: 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
```
示例 1:

输入: [2,4,1], k = 2
输出: 2
解释: 在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2 。
示例 2:

输入: [3,2,6,5,0,3], k = 2
输出: 7
解释: 在第 2 天 (股票价格 = 2) 的时候买入，在第 3 天 (股票价格 = 6) 的时候卖出, 这笔交易所能获得利润 = 6-2 = 4 。
     随后，在第 5 天 (股票价格 = 0) 的时候买入，在第 6 天 (股票价格 = 3) 的时候卖出, 这笔交易所能获得利润 = 3-0 = 3 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv
```
  


思路一： 暴力递归  O(k*N^2)


```python
def maxProfit(k, prices):
    cache = {}
    def dp(buy_day, k):
        if buy_day >= len(prices) or not k: return 0
        if (buy_day, k) in cache: return cache[(buy_day, k)]
        
        minPri = prices[buy_day]
        maxPro = 0
        for sell_day in range(buy_day+1, len(prices)):
            minPri = min(minPri, prices[sell_day])
            maxPro = max(maxPro, dp(sell_day+1, k-1) + prices[sell_day] - minPri)
        cache[(buy_day, k)] = maxPro
        return maxPro
    return dp(0, k)
```


```python
maxProfit(2, [1,10,2,100])
```




    107



铁定会超时，但是非常容易想到
  
思路二: DP  
  
1. 定义状态：`mp[i][k][j]`: 表示第 i 天可交易次数 k 以及 是否持股 可以获得的最大利润      
  
2. 状态转移方程    
```
mp[i][k][0] = max(mp[i-1][k][0], mp[i-1][k][1] + prices[i])
mp[i][k][1] = max(mp[i-1][k][1], mp[i-1][k-1][0] - prices[i])
```  
初试状态：  
```
mp[-1][k][0] = 0    表示还没开始时，最大利润为0
mp[-1][k][1] = -inf  表示未开始时，用负无穷表示不可能持股
mp[i][0][0] = 0     表示 k = 0 时，不允许交易，最大利润为0
mp[i][0][1] = -inf   表示 k = 0 时，不允许交易，负无穷表示不能持股
```


```python
def maxProfit(k, prices):
    if not prices or not k: return 0
#     if k > len(prices)//2:
#         maxPro = 0
#         for sell_day in range(1, len(prices)):
#             maxPro = maxPro + max(prices[sell_day] - prices[sell_day-1], 0)
#         return maxPro
    
    mp = [[[0, float('-inf')] for _ in range(k+1)] for _ in range(len(prices))]
    for i in range(len(prices)):
        for kk in range(k, 0, -1):
            mp[i][kk][0] = max(mp[i-1][kk][0], mp[i-1][kk][1] + prices[i])
            mp[i][kk][1] = max(mp[i-1][kk][1], mp[i-1][kk-1][0] - prices[i])
    return mp[len(prices)-1][k][0]
```

同时，上述代码在 `k>len(prices)//2` 时会超内存，因为一次交易包括买入的一天和卖出的一天，即一次交易至少需要两天。当 `k>len(prices)//2` 时，相当于不限交易次数，即Leet Code 122 问题。而此时最优方案是贪心。所以再增加上述注释的代码即可。

```
小结：思路二代码虽不是最优空间复杂度和时间复杂度，却是能解决股票问题的通用模版  
```
