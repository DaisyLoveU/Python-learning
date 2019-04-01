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
额外空间$O(m+n)$，时间复杂度$O(m+n)$

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
        # 循环结束后，可能 nums1 与 nums2 长短差比较大，导致len(s) 可能小于 m+1
        # 接下来要判断是哪个列表导致循环结束，
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
  
**分析：**  
空间复杂度$O(m+n)$, 时间复杂度$O(m+n)$
  
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
        # list.sort() 的时间复杂度为 O(nlogn)
        num.sort()
        l = len(num)
        if l % 2 == 0:
            return (num[int(l/2)] + num[int((l/2) - 1)]) / 2
        else:
            return num[int((l - 1)/ 2)]
```
**结果：**  
![](./img/leetcode4_res_3.png)  
  
**分析：**  
空间复杂度$O(m+n)$, 时间复杂度$O(nlogn)$, 但是比大多程序得分高，原因是测试用例太少，过于简单。  
  
**思路四：**  

以上思路均不能达到时间复杂度$O(log(m+n))$。很明显，只有使用分治，才可能有 $log$ 的时间复杂度。**可以将此题理解为求两个有序的 array 的第k小的数。  
由于 array 有序，可以每趟可以排除 k//2 个元素。**  
  
假设 A = A0,A1,A2,A3,A4... B = B0,B1,B2,B3,B4...  现寻找第7小的数：  
  
比较 A 和 B 中的第 7//2=3 个元素，若 A2 < B2, 则  
最多有 2 个元素(B0,B1)小于 A0，  
最多有 3 个元素(B0,B1,A0)小于 A1，  
最多有 4 个元素(B0,B1,A0,A1)小于 A2，  
也就是说此时 A0,A1,A2 即 A 中前 7//2 个数不可能是第 7 小的元素，进而排除之。  
  
对于 B0, 可能存在 A0 < A1 < A2 < A3 < A4 < A5 < B0，  
对于 B1, 可能存在 A0 < A1 < A2 < A3 < A4 < B0 < B1，  
对于 B2, 可能存在 A0 < A1 < A2 < A3 < B0 < B1 < B2，所以 B0,B1,B2均可能为第 7 小的元素。即只能排除A0,A1,A2。接下来问题转换成寻找 A = A3,A4...; B = B0,B1,B2,B3,B4...中第 (7 - 7//2) 小的数。**得到 k 的更新公式为 k = k - k//2**  
  
算法每趟都是比较第 k//2 个元素，假设每次都是 A 减少元素，可能出现 k = 1，这时返回 min(A[0], B[0])； 也可能出现数组长度小于 k//2 的情况，这时将指针指向数组最后一个元素即可。这时排除过元素后会出现空列表，那么可直接返回非空列表的第 (k - len(array)) 个元素。  
  
综上，无论是找第奇数个还是第偶数个小的数，对算法并没有影响，而且在算法进行中，k 的值都有可能有奇有偶，最终都会变为 1 或者出现一空个数组，直接返回结果。  
  
所以可采用递归的思路，为了防止数组长度小于 k // 2 ，所以每次比较 min ( k // 2, len (array) ) 对应的数字，把小的那个对应的数组的数字排除，将两个新数组进入递归，并且 k 要减去排除的数字的个数。递归出口就是当 k = 1 或者其中一个数字长度是 0 了。  
  
问题：为什么要从k/2开始寻找，依旧假设 k = 7, 可以比较A0 和 B6的关系么，可以这样做，但是明显的问题出现在如果A0 > B6，那么第 7 小的数应该存在于 B7 和A0 中。
  
如果A0 < B6，这个时间可能性就很多了，比如A0 < A1 < A2 < A3 < A4 < B0 < B1，各种可能，无法排除元素，所以还是要从 k//2 开始寻找。  
  
```Python
class Solution:
    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        n = len(nums1) + len(nums2)
        res = self.find_k(nums1, nums2, n//2+1)
        if n%2: return res/1
        return (self.find_k(nums1, nums2, n//2) + res) /2
    
    def find_k(self,A,B,k):
        if len(A) == 0: return B[k-1]
        if len(B) == 0: return A[k-1]
        if k == 1: return min(A[0], B[0])
#         make sure A is the smaller array
        if len(A) > len(B): 
            A,B = B,A
            return self.find_k(A, B, k)
#         in case of out of range of list due to k//2 > len(A)
        p = min(k//2, len(A))
        if A[p-1] < B[p-1]:
            return self.find_k(A[p:], B, k-p)  
        else: 
            return self.find_k(A, B[p:], k-p)

#     改进，使用列表内部index移动，而非切片

```
**结果：**  
![](./img/leetcode4_res_4.png)




  
  
### 思路五摘于leetcode missmary  


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
