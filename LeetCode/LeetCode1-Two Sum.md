<b> Enjoy coding!</b>

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **exactly** one solution, and you may not use the same element twice.  



**Example**:  

Given nums = [2, 7, 11, 15], target = 9,  
Because nums[0] + nums[1] = 2 + 7 = 9,  
return [0, 1].  



> 首先是题意 'assume that each input would have **exactly** one solution' ，理解为 array 只有一组正确解。  
 ‘you may not use the same element twice’，应该理解为返回的indices不可以重复。  
- 第一种思路，两次循环:
```Python
class Solution:
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        for i in range(len(nums)-1):
            for j in range(i+1, len(nums)):
                if nums[i] + nums[j] == target:
                    return [i, j]
```
结果：[](https://github.com/DaisyLoveU/Python-learning/blob/master/LeetCode/img/leetcode1_res_01.png)  
**分析**：  
两次循环，时间复杂度 $O(n^{2})$ ，没有额外的空间复杂度。

- 第二种思路, 构建hashtable，将第二次循环（$ O(n) $）变成hashtable的查找键值对（$O(1)$）;  
hashtable的key和value分别为target-num和index of num

```Python
class Solution:
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        hashtable = {}
        for i,num in enumerate(nums):
            if num in hashtable:
                return [hashtable[num], i]
            else:
                hashtable[target-num] = i
```
结果： [](https://github.com/DaisyLoveU/Python-learning/blob/master/LeetCode/img/LeetCode1_res_2.png)  
**分析**：   
一次循环，时间复杂度$O(n)$, 构建了hashtable，有额外的$O(n)$空间复杂度。
