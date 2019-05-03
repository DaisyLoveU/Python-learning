
给定一个数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口 k 内的数字。滑动窗口每次只向右移动一位。

返回滑动窗口最大值。

**示例：**  
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3  
输出: [3,3,5,5,6,7]   
解释:   


 ```
滑动窗口的位置                最大值
 [1  3  -1] -3  5  3  6  7        3  
 1 [3  -1  -3] 5  3  6  7         3  
 1  3 [-1  -3  5] 3  6  7         5  
 1  3  -1 [-3  5  3] 6  7         5  
 1  3  -1  -3 [5  3  6] 7         6  
 1  3  -1  -3  5 [3  6  7]        7 
 ```

**思路一：暴力法**  
时间复杂度: O(Nk)  
空间复杂度: O(1)


```python
class Solution(object):
    def maxSlidingWindow(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        if not nums:
            return []
        res = []
        for i in range(len(nums)-k+1):
            res.append(max(nums[i:i+k]))
        return res
```

**思路二：使用heapq**  
维护一个大小为k的大顶堆，每次滑动窗口后，pop窗口第一个值，push新进窗的值，并调整堆。
时间复杂度:O(Nlogk)

代码么得~~~

**思路三：使用deque优化**  
首先，遍历nums的前k个元素，仅将最大值的索引放入window中（注意：window只存储索引）  
然后，比较新来的值x和window中最右边的值，若x大，则弹出window最右边的值，否则不需要x。可以得到长度为k的第一个元素为最大元素索引的双端队列。  
现在，我们遍历nums的第k到最后一个元素：并且：  
1. 将window中最左侧索引值添加到res  
2. 随着窗口的移动，检查window中最左侧索引是否有效
3. 比较新来的值x和window最右边的值，若x大，则弹出window最右边的值
5. 在window中添加i  

时间复杂度：O(N)  
空间复杂度：O(1)


```python
class Solution(object):
    def maxSlidingWindow(self, nums, k):
        if not nums or len(nums) == 0:
            return []

        window, res = collections.windowue(), []
        
        for i in range(k):
            while window:
                if nums[i] > nums[window[-1]]:
                    window.pop()
                else:
                    break
            window.append(i)
            
        for i in range(k, len(nums)):
            res.append(nums[window[0]])
            if window[0] < i - k + 1:
                window.popleft()
            
            while window:
                if nums[i] > nums[window[-1]]:
                    window.pop()
                else:
                    break
            window.append(i)
            
        res.append(nums[window[0]])
        return res
```

上面代码思路清晰，但是比较冗长，现可将两个for loop整合到一起


```python
class Solution(object):
    def maxSlidingWindow(self, nums, k):
        if not nums or not len(nums):
            return []
        window, res = [],[]
        for i,x in enumerate(nums):
            if i >= k and window[0] < i - k + 1:  
                # 随着窗口的移动，检测window[0]是否还在窗内。i-k+1是窗口的左边界
                window.pop(0)
            while window and nums[window[-1]] <= x:  
                # 核心思想，队列存放最大值的索引，小于x的删除掉
                window.pop()
            window.append(i)
            if i >= k - 1:  
                # 当遍历到第k个元素时，即处理过第一个窗口后，开始把每个窗内最大元素添加至res
                res.append(nums[window[0]])
        return res
```
