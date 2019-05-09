
给定一个大小为 n 的数组，找到其中的众数。众数是指在数组中出现次数大于 ⌊ n/2 ⌋ 的元素。

你可以假设数组是非空的，并且给定的数组总是存在众数。  

示例 1:  
  
输入: [3,2,3]  
输出: 3  
  
示例 2:

输入: [2,2,1,1,1,2,2]  
输出: 2  

思路零：  
暴力  O(n^2)


```python
class Solution:
    def majorityElement(self, nums): 
        majority_count = len(nums) // 2
        for i in nums:
            count = sum(1 for j in nums if i == j)
            if count > majority_count:
                return i
```

思路一：  
使用map来对nums中对元素计数，返回出现次数最多的元素  
时间复杂度和空间复杂度都是O(n)


```python
class Solution:
    def majorityElement(self, nums):
        dyct = {}
        for i in nums:
            dyct[i] = dyct.get(i, 0) + 1
        for k,v in dyct.items():
            if v > len(nums)//2:
                return k
```

思路二：  
对nums排序，则众数一定是nums[len(nums)//2]  
时间复杂度O(nlogn)  
空间复杂度O(1)


```python
class Solution:
    def majorityElement(self, nums):
        nums.sort()
        return nums[len(nums)//2]
```

思路三：  
遍历nums，使用list.count()计算每个元素出现的次数  
时间复杂度O(n)  
空间复杂度O(n)


```python
class Solution:
    def majorityElement(self, nums):
        numss = set(nums)
        for i in numss:
            if nums.count(i) > len(nums)//2:
                return i
```

思路四： 
  
比较出名的Boyer-Moore众数(majority number) 问题  
  
BM算法还可以用于分治，比如nums特别大，可以将nums切分为多个小数组，分别使用BM算法求小数组的众数，然后再整合到一起，得出nums的众数。
  
在数组中找到两个不相同的元素并删除它们，不断重复此过程，直到数组中元素都相同，那么剩下的元素就是主要元素。

这个算法的妙处在于不直接删除数组中的元素，而是利用一个计数变量.  

时间复杂度O(n)  
空间复杂度O(1)


```python
class Solution:
    def majorityElement(self, nums):        
        count,major=0,0
        for n in nums:
            if count == 0:
                major = n
            if major == n:
                count += 1
            else:
                count -= 1
        return major
```
