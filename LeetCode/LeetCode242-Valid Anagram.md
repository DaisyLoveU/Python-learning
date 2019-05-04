
给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的一个字母异位词。  
示例 1:

输入: s = "anagram", t = "nagaram"
输出: true  

示例 2:

输入: s = "rat", t = "car"
输出: false  

说明:  
你可以假设字符串只包含小写字母。  
  
  
思路一：排序并比较


```python
class Solution(object):
    def isAnagram(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        return sorted(s) == sorted(t)
```

思路二：使用map计数并比较


```python
class Solution(object):
    def isAnagram(self, s, t):
        ds,dt = {},{}
        for i in s:
            ds[i] = ds.setdefault(i, 0) + 1
        for i in t:
            dt[i] = dt.setdefault(i, 0) + 1
        return ds == dt
```

变种，只包含小写字母，则可用长度为26的数组记录每个字母出现的次数


```python
class Solution(object):
    def isAnagram(self, s, t):
        ds,dt = [0]*26,[0]*26
        for i in s:
            ds[ord(i) - ord('a')] += 1
        for i in t:
            dt[ord(i) - ord('a')] += 1
        return ds == dt
```
