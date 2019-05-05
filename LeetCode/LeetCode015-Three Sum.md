
给定一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？找出所有满足条件且不重复的三元组。  
注意：答案中不可以包含重复的三元组。

例如, 给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]  

## 面试中 优先考虑双指针，并使用set保存res，即可以不考虑去重

**思路一：利用twoSum**  
遍历nums,然后转换为twoSum问题，但是要排除重复结果，判重操作时间O(n)，twoSum本身时间O(n)，空间O(n)，总体时间复杂度O(n^3)，空间复杂度O(n)


```python
class Solution(object):  # 此法也超时
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        def twoSum(nums, target):
            lookup = {}
            for num in nums:
                if target - num in lookup:
                    if (-target ,target - num, num) not in res:
                        res.append((-target ,target - num, num))
                lookup[num] = target - num

        n = len(nums)
        nums.sort()
        res = []
        for i in range(n):
            twoSum(nums[i+1:], 0-nums[i])
        return [list(i) for i in res]
```

**思路二：暴力法**  
先给nums排序，满足结果从小到大排列，三层循环


```python
class Solution(object):
    def threeSum(self, nums):
        n = len(nums)
        res = []
        nums.sort()
        for i in range(n):
            for j in range(i,n):
                for k in range(j,n):
                    if nums[i] + nums[j] + nums[k] == 0 and j != i and k != j and k != i: 
                        curRes = [nums[i],nums[j],nums[k]]
                        if curRes not in res:
                            res.append(curRes)
    
        return res
```

**思路三：优化二**  
两个for loop枚举a，c，则b=-a-c,然后去dyct中查询-a-c是否存在。利用set保存res，达到去重效果。    
整体时间复杂度O(n^2)，空间复杂度O(n)


```python
def threeSum(nums):
    if len(nums) < 3:
        return []
    nums.sort()
    res = set()
    for i,a in enumerate(nums[:-2]):
        if i >= 1 and a == nums[i-1]:
#             两个连续的值相等时，已经处理过了，跳过此次循环,
            continue
        if v > 0: break  ##因为已经排序了，所以到大于零的位置，之后的数字都是大于零的，肯定没有符合条件的组合了

        dyct = {}
        for c in nums[i+1:]:
            if c not in dyct:
                dyct[-a-c] = None
            else:
                res.add((a, -a-c, c))
    return list(map(list, res))
```


```python
threeSum([-1, 0, 1, 2, -1, -4])
```




    [[-1, -1, 2], [-1, 0, 1]]



**思路四：双指针**  
排序好nums，遍历nums，对nums[i+1:]，使用两个指针l,r分别从左右开始，计算当前数nums[i]和nums[l]，nums[r]之和s，若s小于0，由于nums是排好序的，只需l右移，若s大于0则只需r左移，若等于0，则将nums[i],nums[l],nums[r]存入res列表，并左移r 右移l继续比较。直至两个指针发生碰撞。  
注意点：对于相邻重复的元素，需要跳过。  
由于需要遍历nums，且双指针遍历nums[i+1:]，则总体```时间复杂度为O(n^2)，空间复杂度为O(n)```  


```python
def threeSum(nums):
    '''
    使用set保存res，不需要考虑判重，简单直接，思路清晰
    '''
    res = set()
    nums.sort()
    for i in range(len(nums)-2):
        l,r = i+1,len(nums)-1
        while l < r:
            s= nums[i] + nums[l] + nums[r]
            if s < 0:
                l += 1
            elif s >0:
                r -= 1
            else:
                res.add((nums[i],nums[l],nums[r]))
                l += 1
                r -= 1
    return [list(i) for i in res]
```


```python
threeSum([-1, 0, 1, 2, -1, -4])
```




    [[-1, -1, 2], [-1, 0, 1]]




```python
threeSum([0,0,0,0,0,0,0,0,0])
```




    [[0, 0, 0]]



充分考虑nums结构，进行判重，减少操作，减少时间，加大代码复杂度  
且利用排序好的nums，可知当nums[i]>0时，i，i+1，i+2三个元素都大于零，即i以后的元素不存在解。  


```python
def threeSum(nums):
    '''
    使用list保存res,在过程中判重。
    '''
    res = []
    nums.sort()
    for i in range(len(nums)-2):
        if i>0 and nums[i] == nums[i-1]: # 两个连续的值相等时，已经处理过了，跳过此次循环,
            continue
        if nums[i]>0: break #因为已经排序了，所以到大于零的位置，之后的数字都是大于零的，肯定没有符合条件的组合了
        l,r = i+1,len(nums)-1
        while l < r:
            s= nums[i] + nums[l] + nums[r]
            if s < 0:
                l += 1
            elif s >0:
                r -= 1
            else:
                res.append((nums[i],nums[l],nums[r]))
                while l < r and nums[l] == nums[l+1]: # 再去重
                    l += 1
                while l < r and nums[r] == nums[r-1]:
                    r -= 1
                l += 1
                r -= 1
    return res
```


```python
threeSum([-1, 0, 1, 2, -1, -4])
```




    [(-1, -1, 2), (-1, 0, 1)]


