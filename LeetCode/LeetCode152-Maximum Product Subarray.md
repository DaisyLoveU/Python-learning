
给定一个整数数组 nums ，找出一个序列中乘积最大的连续子序列（该序列至少包含一个数）。
```
示例 1:

输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
示例 2:

输入: [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。
```  

  
思路：dp  
  
1. 定义状态  
dp[i] 表示 从 0~i ，选取nums[0:i+1]个元素的最大和  
  
2. 状态转移方程  
dp[i] = max(dp[i-1] * nums[i], nums[i])  
  
显然，上述思想的状态转移方程忽略了有偶数个负数的情况，eg: [2,3,-2,4,-5]  
  
因此，考虑到需要状态需要保存正数时的最大值，还要保存负数的最小值，这样才能保证不忽略负数情况  
  
因此，重新定义状态  
`dp[i][0] 表示正数的最大值  
dp[i][1] 表示负数的最小值`  
  
重新定义状态转移方程  
`dp[i][0] = dp[i-1][0] * nums[i] if nums[i] > 0 else
        dp[i-1][1] * nums[i]  
dp[i][1] = dp[i-1][1] * nums[i] if nums[i] > 0 else
        dp[i-1][0] * nums[i]`


```python
class Solution:
    def maxProduct(self, nums) -> int:
        if not nums: return 0
        dp = [[0 for _ in range(2)] for _ in range(len(nums))]
        dp[0] = [nums[0],nums[0]]
        res = nums[0]
        for i in range(1, len(nums)):
            dp[i][0] = max(dp[i-1][0] * nums[i],dp[i-1][1] * nums[i], nums[i])
            dp[i][1] = min(dp[i-1][1] * nums[i],dp[i-1][0] * nums[i], nums[i])
            res = max(res, dp[i][0])
        return res
```
