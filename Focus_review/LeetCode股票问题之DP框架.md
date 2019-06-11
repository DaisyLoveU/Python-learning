
### 读[LeetCode 股票问题的一种通用解法](https://mp.weixin.qq.com/s?__biz=MzU0MDg5OTYyOQ==&mid=2247484032&idx=1&sn=cafed934bd5d8a733de3b3bc675e6a19&chksm=fb3362c2cc44ebd4c5eb7bc41baf540f55ee6f4d855bfcca7ebc4a5d2fea9a3b827bef5c18eb&scene=21#wechat_redirect)之后，我便写了[一个框架闯天涯](https://blog.csdn.net/zz_daisy/article/details/91358530)，现来记录我直接使用三维DP框架怎么解决 Leet Code 的六个股票问题

通过上篇文章，可以肯定的是，这六道题目一定存在共性，且能够使用一种框架解决。  
  
接下来，我们从最难的也最泛化的 [买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/) 入手  
  
首先，可以想到的做法是暴力。  
  
其次，使用DP，那么我们就要 1. 定义状态  2. 状态转移方程  
  
第一次尝试定义状态：  
  
使用一维数组表示状态
`MP[i]，表示第 i 天的 max profit， MP[n-1] 即为所求。`  
  
写状态转移方程：  
  
每一天都会有三个操作，买入，卖出或不作为。那么 MP[i] 应该等于前一天的最大利润减去第 i 天买入股票 的价格，或者加上第 i 天卖出股票的价格...那么状态转移方程应该是  
```
MP[i] = MP[i-1] + (-prices[i])  买入
        MP[i-1] + prices[i]     卖出
```  
  
那么，由题，我们只能完成一次交易后，再进行下一次交易。即只有不持股的时候才能买入，持股的时候才能卖出。可以发现一维状态会丢失很多信息，我们需要增加状态的维度，来表示更多信息：  
`MP[i][j], j = 0 或 1 来表示是否持股， 表示第 i 天持股或未持股的最大利润`  
  
状态转移方程就可以这么写：  
```
MP[i][0] = max(MP[i-1][0], MP[i-1][1] + prices[i])  # 括号前者表示前一天不作为，后者表示前一天持股并卖出
MP[i][1] = max(MP[i-1][1], MP[i-1][0] - prices[i])  # 括号前者表示前一天不作为，后者表示前一天未持股并买入
```  
  
可以得到，增加状态的一个维度，可以让状态具备更多信息。  
那么，可以发现，现在的状态并没有记录交易次数，因此，我们干脆再增加状态一个维度，让其表示可交易次数：  
`MP[i][k][j], k = 0~K，表示第 i 天持股或未持股且能交易 k 次的最大利润`  
  
状态转移方程:
```
for i: 0~n-1
  for k: K~0
    MP[i][k][0] = max(MP[i-1][k][0], MP[i-1][k][1] + prices[i])
    MP[i][k][1] = max(MP[i-1][k][1], MP[i-1][k-1][0] - prices[i])  我们定义在买入的时候减少交易次数
```  
  
上述状态以及状态转移方程就是此类问题的标准框架了，对于不同的问题，稍加修改即可。  
  
下面来确定初始状态  
```
MP[-1][k][0] = 0    表示还没开始时，最大利润为0
MP[-1][k][1] = -inf  表示未开始时，用负无穷表示不可能持股
MP[i][0][0] = 0     表示 k = 0 时，不允许交易，最大利润为0
MP[i][0][1] = -inf   表示 k = 0 时，不允许交易，负无穷表示不能持股
```  
  
有了之上的分析，我们就可以码出代码咯


```python
def maxProfit(k, prices):
    if not prices or not k: return 0
    
    mp = [[[0, float('-inf')] for _ in range(k+1)] for _ in range(len(prices))]
    for i in range(len(prices)):
        for kk in range(k, 0, -1):
            mp[i][kk][0] = max(mp[i-1][kk][0], mp[i-1][kk][1] + prices[i])
            mp[i][kk][1] = max(mp[i-1][kk][1], mp[i-1][kk-1][0] - prices[i])
    return mp[len(prices)-1][k][0]
```

在 leetcode 运行后，发现竟然超时了。  
  
原因是，k 非常大的时候，就相当于没有交易次数限制了，此时使用贪心算法是最优的。  
  
k 多大的时候相当于没有交易次数限制呢？  
一次交易最少需要买入一天和卖出的一天，即共需 2 天，当 k >= n//2 时，就相当于没有交易次数限制咯~  
  
我们使用贪心处理没有交易限制的情况：


```python
def maxProfit(k, prices):
    if not prices or not k: return 0
    if k > len(prices)//2:
        maxPro = 0
        for sell_day in range(1, len(prices)):
            maxPro = maxPro + max(prices[sell_day] - prices[sell_day-1], 0)
        return maxPro
    
    mp = [[[0, float('-inf')] for _ in range(k+1)] for _ in range(len(prices))]
    for i in range(len(prices)):
        for kk in range(k, 0, -1):
            mp[i][kk][0] = max(mp[i-1][kk][0], mp[i-1][kk][1] + prices[i])
            mp[i][kk][1] = max(mp[i-1][kk][1], mp[i-1][kk-1][0] - prices[i])
    return mp[len(prices)-1][k][0]
```

对于 [121.买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)  
此时 K = 1, 直接套 DP 状态模板，刷刷刷~ ~ ~ 直接出答案，优化什么的之后再说呗


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

对于[122.买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)  
此时 K = infinity，我们认为 k-1 ≈ k，则可见 k 对状态不起作用，去掉这一维度，直接使用模版中的贪心部分，刷刷刷~ ~ ~直接出答案！


```python
def maxProfit(prices):
    mp = [[0, float('-inf')] for _ in range(len(prices))]
    for i in range(len(prices)):
        mp[i][0] = max(mp[i-1][0], mp[i-1][1] + prices[i])
        mp[i][1] = max(mp[i-1][1], mp[i-1][0] - prices[i])
    return mp[len(prices)-1][0]
```

这题，也可以套用模版中的贪心部门，dong~ ba~ chua~ 直接出答案


```python
def maxProfit(prices):
    if not prices: return 0
    maxPro = 0
    for sell_day in range(1, len(prices)):
        maxPro = maxPro + max(prices[sell_day] - prices[sell_day-1], 0)
    return maxPro
```

对于[123.买卖股票的最佳时机 III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/)  
此时 K = 2, 直接套模板，cua~ cua~ cua~ 直接出答案！


```python
def maxProfit(prices):
    k = 2
    if not prices: return 0
    
    mp = [[[0, float('-inf')] for _ in range(k+1)] for _ in range(len(prices))]
    for i in range(len(prices)):
        for kk in range(k, 0, -1):
            mp[i][kk][0] = max(mp[i-1][kk][0], mp[i-1][kk][1] + prices[i])
            mp[i][kk][1] = max(mp[i-1][kk][1], mp[i-1][kk-1][0] - prices[i])
    return mp[len(prices)-1][k][0]
```

对于[309.最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)  
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

对于[714.买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)  
此时 K = infinity, 每次交易需要缴纳手续费，只需在买入或卖出的时候减去手续费即可. 套用模版，diu~ diu~ diu~ 出答案~


```python
class Solution:
    def maxProfit(prices, fee) -> int:
        mp = [[0, float('-inf')] for _ in range(len(prices))]
        for i in range(len(prices)):
            mp[i][0] = max(mp[i-1][0], mp[i-1][1] + prices[i])
            mp[i][1] = max(mp[i-1][1], mp[i-1][0] - prices[i] - fee)
        return mp[len(prices)-1][0] 
```

总结，leetcode第188题是最具范性的股票问题，自己根据此题推出三维 DP 状态以及状态转移方程模板，只需稍微修改即可通过其他股票问题。
