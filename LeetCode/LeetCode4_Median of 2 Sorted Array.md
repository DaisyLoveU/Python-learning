### Leet Code 4 - Median of 2 Sorted Array

There are two sorted arrays nums1 and nums2 of size m and n respectively.  

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).  

You may assume nums1 and nums2 cannot be both empty.  
**Example 1:**  

nums1 = [1, 3]  
nums2 = [2]  

The median is 2.0  

**Example 2:**  

nums1 = [1, 2]  
nums2 = [3, 4]  

The median is (2 + 3)/2 = 2.5  

**思路一：**
简单粗暴使用归并排序，将 nums1 和 nums2 所有元素按照归并排序合并时的思想合并成一个列表，然后根据列表总长度输出。  
**代码：**  
```Python
class Solution:
    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        l1,l2 = len(nums1),len(nums2)
        m = (l1 + l2) // 2
        s = []
        i = j =0
        while i < l1 and j < l2:
            if nums1[i] < nums2[j]:
                s.append(nums1[i])
                i += 1
            else:
                s.append(nums2[j])
                j += 1
        s += nums1[i:]
        s += nums2[j:]

        if (l1 + l2) % 2:
            return s[m]/1
        else:
            return (s[m] + s[m-1])/2
```
**结果：**  
![](./img/leetcode4_res_1.png)  

**分析：**  
额外空间$O(n)$，时间复杂度$O(n)$?

**思路二：**  
改进思路一。一中是将两个列表完全合并，稍微改进一些，是只合并前 $(len(nums1) + (len(nums2)) / 2 + 1$ 个元素  

```Python
class Solution:
    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        l1,l2 = len(nums1),len(nums2)
        m = (l1 + l2) // 2
        s = []
        i = j = 0
        while i < l1 and j < l2 and len(s) < m+1:
            if nums1[i] < nums2[j]:
                s.append(nums1[i])
                i += 1
            else:
                s.append(nums2[j])
                j += 1

        if i == l1 and len(s) < m+1:
            for ii in range(m+1-len(s)):
                s.append(nums2[j+ii])

        if j == l2 and len(s) < m+1:
            for jj in range(m+1-len(s)):
                s.append(nums1[i+jj])
                
        if (l1 + l2) % 2:
            return s[-1] / 1
        else:
            return (s[-1] + s[-2]) / 2
```
**结果：**    
![](./img/leetcode4_res_2.png)  

**思路三：**  
很崩溃，找到官方的快解，速度好快呀。不知道为什么list.sort()会比归并快。  

```Python

class Solution:
    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        num = nums1 + nums2
        num.sort()
        l = len(num)
        if l % 2 == 0:
            return (num[int(l/2)] + num[int((l/2) - 1)]) / 2
        else:
            return num[int((l - 1)/ 2)]
```
**结果：**  
![](./img/leetcode4_res_3.png)
## 分析：  
# 可能根据电脑性能和网速波动，使得运行时间波动！！！嗯，一定是这样的

# 思路摘于leetcode missmary  


[大神操作](https://leetcode.com/problems/median-of-two-sorted-arrays/discuss/2481/Share-my-O(log(min(mn))-solution-with-explanation "悬停显示")  

**时间复杂度为$O(log(min(m,n)))$ but beats 70+**
```Python3
class Solution:
    def findMedianSortedArrays(self,A, B):
        m, n = len(A), len(B)
        if m > n:
            A, B, m, n = B, A, n, m
        if n == 0:
            raise ValueError

        imin, imax, half_len = 0, m, (m + n + 1) // 2
        while imin <= imax:
            i = (imin + imax) // 2
            j = half_len - i
            if i < m and B[j-1] > A[i]:
                # i is too small, must increase it
                imin = i + 1
            elif i > 0 and A[i-1] > B[j]:
                # i is too big, must decrease it
                imax = i - 1
            else:
                # i is perfect

                if i == 0: max_of_left = B[j-1]
                elif j == 0: max_of_left = A[i-1]
                else: max_of_left = max(A[i-1], B[j-1])

                if (m + n) % 2 == 1:
                    return max_of_left

                if i == m: min_of_right = B[j]
                elif j == n: min_of_right = A[i]
                else: min_of_right = min(A[i], B[j])

                return (max_of_left + min_of_right) / 2.0
```
