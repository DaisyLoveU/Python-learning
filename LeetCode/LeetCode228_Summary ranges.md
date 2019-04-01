Given a sorted integer array without duplicates, return the summary of its ranges.  
**Example 1:**  
Input:  [0,1,2,4,5,7]  
Output: ["0->2","4->5","7"]  
Explanation: 0,1,2 form a continuous range; 4,5 form a continuous range.  
**Example 2:**  
Input:  [0,2,3,4,6,8,9]  
Output: ["0","2->4","6","8->9"]  
Explanation: 2,3,4 form a continuous range; 8,9 form a continuous range.  


**思路一：**  
为列表末尾添加无穷大，遍历一遍列表，对于每个下标```i```，若其对应的元素与下一个下标```i+1```对应的元素相差一，则用临时列表保存这两个元素；出现间断时，通过判断临时列表的长度，确认当前元素之前是否存在连续的元素，然后将结果添加至res列表。每次循环后，需要重置临时列表。
```python
class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:
        res = []
        tmp = []
        nums.append(float("inf"))
        for i in range(len(nums)-1):
            
            if nums[i+1] - nums[i] == 1:
                tmp += [nums[i], nums[i+1]]
            else:
                if len(tmp) > 1:
                    res.append(str(tmp[0])+"->"+str(tmp[-1]))
                else:
                    res.append(str(nums[i]))
                tmp = []
        return res
```

**结果：**  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190401220623260.png)

**分析：**  
空间复杂度过高，每次为temp列表添加0或2个元素

**思路二：**
优雅地使用二维列表暴力地添加元素。
```python
class Solution(object):
    def summaryRanges(self, nums):
        ranges = []
        for i in nums:
            if not ranges or i > ranges[-1][-1] + 1:
                ranges += [],
            ranges[-1][1:] = i,
        return ['->'.join(map(str, r)) for r in ranges]   
```
**结果：**  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019040122373064.png)


**分析：**  
关于两个逗号的使用
> ranges += [],  
> ranges[-1][1:] = i,  
  
首先看没有这两个逗号会怎样：
> ranges += []  # ranges列表只会添加```[]```里面的值，即nothing！  
> ranges[-1][1:] = i  # 该语句会报错，因为赋值给切片的值不是```iterable!```  


逗号可以把等号右边的内容变成```tuple```，以上语句相当于下面的语句：
> ranges.append([])  
> or   
> ranges += [[]]  
> ranges[-1][1:] = [i]  # 切片的赋值，只能用```iterable```对象  
  
那么为什么不用普通的语句呢？因为作者比较过两者之间的速度，使用逗号的话，代码会更优雅，更快！
