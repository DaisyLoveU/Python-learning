
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
```
示例 1:

输入: [7,1,5,3,6,4]  
输出: 7  
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。  
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。  
  
示例 2:  

输入: [1,2,3,4,5]  
输出: 4  
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。  
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。  
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。  
     
示例 3:  

输入: [7,6,4,3,1]  
输出: 0  
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。  
```

思路一：最优解法贪心  
当明天股票价格高于今天时，就买入，并在明天卖出


```python
class Solution:
    def maxProfit(self, prices):
        res = 0
        for i in range(len(prices)-1):
            if prices[i] < prices[i+1]:
                res += prices[i+1] - prices[i]
        return res
```

时间复杂度 O(n)  
空间复杂度 O(1)

思路二：[一个框架闯天涯](https://blog.csdn.net/zz_daisy/article/details/91358530)中的思路和框架


```python
def maxProfit(prices):
    
    def dp(buy):
        if buy >= len(prices): return 0
        if buy in cache: return cache[buy]
        
        maxPro = 0
        minPri = prices[buy]
        for sell in range(buy+1, len(prices)):
            minPri = min(minPri, prices[sell])
            maxPro = max(maxPro, dp(sell+1) + prices[sell] - minPri)
        cache[buy] = maxPro
        return maxPro
    
    cache = {}
    return dp(0)
```

时间复杂度 O(n^2)  
空间复杂度 O(n^2) 递归深度

思路三： 套用[股票问题的DP框架](https://blog.csdn.net/zz_daisy/article/details/91411704)  
  
k = infinity， k ≈ k-1 则  
```
mp[i][0] = max(mp[i-1][0], mp[i-1][1] + prices[i])
mp[i][1] = max(mp[i-1][1], mp[i-1][0]-prices[i])
```  


```python
def maxProfit(prices):
    mp = [[0, float('-inf')] for _ in range(len(prices))]
    for i in range(len(prices)):
        mp[i][0] = max(mp[i-1][0], mp[i-1][1] + prices[i])
        mp[i][1] = max(mp[i-1][1], mp[i-1][0] - prices[i])
    return mp[len(prices)-1][0]
```

思路四：优化思路三  
  
可以发现，mp[i][0],mp[i][1] 只与 mp[i-1][0],mp[i-1][1]有关，可以状态压缩  
  
但是不能像 121 题一样只使用两个变量，因为 更新 mp[i][1] 时，mp[i][0]的值已经改变了，所以需要引入一个临时变量


```python
class Solution:
    def maxProfit(self, prices):
        if len(prices) < 2: return 0
        mp_i_0, mp_i_1 = 0, -prices[0]
        for i in range(len(prices)):
            tmp = mp_i_0
            mp_i_0 = max(mp_i_0, mp_i_1 + prices[i])
            mp_i_1 = max(mp_i_1, tmp - prices[i])
        return mp_i_0
```
