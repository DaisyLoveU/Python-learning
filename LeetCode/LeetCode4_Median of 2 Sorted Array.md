### Leet Code 4 - Median of 2 Sorted Array

There are two sorted arrays nums1 and nums2 of size m and n respectively.  

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).  

You may assume nums1 and nums2 cannot be both empty.  



```
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
