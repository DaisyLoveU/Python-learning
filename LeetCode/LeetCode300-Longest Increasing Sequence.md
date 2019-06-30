
给定一个无序的整数数组，找到其中最长上升子序列的长度。
```
示例:

输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
说明:

可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
你算法的时间复杂度应该为 O(n^2) 。
进阶: 你能将算法的时间复杂度降低到 O(nlogn) 吗?

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-increasing-subsequence
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

思路一：暴力法  
  
还是说，不到万不得已，我们不进行暴力法  
  
对于每个数，我们有两种选择方法，选或不选，遍历所有元素，记录最长的上升子序列即可  


思路二：DP  
  
  我们先看示例输入，推出 DP 状态和状态转移方程  
  ```
i      0  1  2  3  4  5    6   7
num  [10, 9, 2, 5, 3, 7, 101, 18]
dp     1  1  1  2  2  3    4   4
  ```  
  
  可以看出，对于任意的 i，dp[i-1] 已经是定值，与 a[i] 后面元素无关  
  且，初始状态 dp[i] =1 ,接下来
  
DP 两步~走~~~  
  
1 定义状态  
  
  dp[i] 表示下标从 0 至 i，选择 a[i] 作为上升子序列的尾部元素，得到的上升子序列的最大长度  
  所以最优解不再是 `dp[n-1]`, 而应该是 `max(dp[0], dp[1], ..., dp[n-1])`  
  
  
2 状态转移方程  
```  
  for i: 0 -> n-1:  
    dp[i] = max{dp[j] + 1} {j: 0 -> i-1 且 a[j] < a[i]}
   ``` 
  
  接下来就可以开心敲代码咯


```python
class Solution:
    def lengthOfLIS(self, nums) -> int:
        if not nums: return 0
        dp = [1 for _ in range(len(nums))]
        
        for i in range(len(nums)):
            for j in range(i):
                if nums[j] < nums[i]:
                    dp[i] = max(dp[i]. dp[j] + 1)
        return max(dp)
```

小伙伴们可以发现，上面　DP 的时间复杂度是 O(n^2)，空间复杂度是 O(n)  
在进阶中，题目让我们最好做到 O(nlogn) 的时间复杂度，那么怎么做到呢？  
  
[请点击后查看大屏幕](https://leetcode-cn.com/problems/search-insert-position/solution/te-bie-hao-yong-de-er-fen-cha-fa-fa-mo-ban-python-/)
