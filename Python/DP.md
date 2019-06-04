
## dynamic programming
  
1. 递归 + 记忆化 ——> 递推  
2. 状态的定义：opy[n], dp[n], fib[n]  
3. 状态转移方程： opt[n] = best_of(opt[n-1], opt(n-2),...)  
4. 最优子结构  
  
  
  
  
![image.png](attachment:image.png)

![image.png](attachment:image.png)

## DP vs 回溯 vs 贪心  
  
- 回溯(递归) ——重复计算  
- 贪心 ——永远局部最优  
- DP ——记录局部最优子结构/多种记录值    回溯+贪心 约等于 DP
