
给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

示例 1:

输入: coins = [1, 2, 5], amount = 11
输出: 3 
解释: 11 = 5 + 5 + 1
示例 2:

输入: coins = [2], amount = 3
输出: -1
说明:
你可以认为每种硬币的数量是无限的。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/coin-change
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

思路一：暴力法  
  
思路二：贪心法  
  
每次都用较大面额的硬币去凑  
  
可能会出错比如给定 coins = [1, 6, 7], amount = 30, 使用贪心的话  
res = 6, 4*7 + 2*1  
而实际使用 5 个6元的硬币即可  


思路三：DP  
  
  硬币问题跟爬楼梯问题有异曲同工之妙，可以想象成给定每次爬楼梯的步数和楼梯的阶数，求最少多少步可以爬完楼梯，类似地，DP两步走起~  
    
1. 定义状态方程  
```DP[i]: 表示 i 阶台阶，最少的步数```  
  
2. 状态转移方程  
```DP[i] = min{DP[i-steps[j]]} + 1, j: 0 -> len(steps)-1```  
且有初始状态 DP[0] = 0  

码代码的时候注意是基于 DP[0], 从 DP[1] 开始形成整个 DP 数组，每次更新 DP[i] 时注意隐形条件：```i >= step[j] ```  
还有就是注意异常情况咯，见代码


```python
class Solution:
    def coinChange(self, coins, amount) -> int:
        if not coins or amount < 0: return -1
        
        dp = [0] + [float("inf") for _ in range(amount)]
        for i in range(len(dp)):
            for coin in coins:
                dp[i] = min(dp[i-coin] + 1, dp[i]) if i >= coin else dp[i]
        return dp[-1] if dp[-1] != float("inf") else -1
```

思路三：完全背包  
  
仔细想想，该问题跟完全背包问题也有些神似，可以理解为 amount 为背包体积，coins 为物品的体积，所求即刚好装满背包的状态下，所需最少的物品数量。
