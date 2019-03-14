
Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.  

**Example 1:**  
Input: "babad"  
Output: "bab"  
Note: "aba" is also a valid answer.  
**Example 2:**  
Input: "cbbd"
Output: "bb"

**思路一：**  
暴力法！通过起始索引 ```i,j```穷举所有可能的子串，判断子串是否为回文，使用 ```length``` 变量记录最大回文子串的长度，若新的回文子串长度大于当前的 ```length``` 则更新 ```length```，并记录当前子串的起始索引。最终通过索引返回最长回文子串。


```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        length = left = right =0
        n = len(s)
        if n<=1: return s
        for i in range(n):
            for j in range(i+1, n+1):
                sub = s[i:j]
                if length < j-i and sub == sub[::-1]:
                    left,right = i,j
                    length = j-i
        return s[left:right]
```

**结果：**  
![](https://github.com/DaisyLoveU/Python-learning/blob/master/LeetCode/img/leetcode5_res_1.png)  

**分析：**  
- 时间复杂度：$O(n^3)$, 假设 $n$ 是输入字符串的长度，则$C_n^2 = n(n-1)/2$ 为子字符串的总数。验证每个子字符串需要$O(n)$的时间，所以运行时间复杂度是$O(n^3)$。
- 空间复杂度：$O(1)$  

  
**思路二：**  
中心扩展方法。我们观察到回文中心的两侧互为镜像。因此，回文可以从它的中心展开，并且只有$2n-1$个这样的中心。

你可能会问，为什么会是$2n-1$个，而不是$n$个中心？原因在于所含字母数为偶数的回文的中心可以处于两字母之间（例如$"abba"$的中心在两个$"b"$之间）。
接下来的关键就是对边界的把握，确保下标不要越界。


```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        length = len(s)
        max_len = start = end = 0
    
        for i in range(length):
            len1 = self.expand(s, i, i) #奇数中心扩散，判断该中心点的最大回文长度
            len2 = self.expand(s, i, i+1) #偶数中心扩散
            max_len = max(len1, len2)
            if max_len > end-start:
                start = i - (max_len -1)//2
                end = i + max_len//2
        return(s[start:end+1])
    
    def expand(self, s, l, r):
        while l >= 0 and r < len(s) and s[l] == s[r]:
            l -= 1
            r += 1
        return r-l-1
```

**结果：**  

![](https://github.com/DaisyLoveU/Python-learning/blob/master/LeetCode/img/leetcode5_res_2.png)

**分析：**  
时间复杂度：$O(n^2)$，遍历中心复杂度为$O(n)$，对于每个中心，从中心展开的时间复杂度为$O(n)$，所以总的时间复杂度为$O(n^2)$.
空间复杂度：$O(1)$.

**思路三：** Manacher algorithm (马拉车算法)  
1). 中心展开法需要按照子串长度的奇偶，分情况处理子串。**为了统一，Manacher算法在每个字符的两端插入一个特殊符号比如 "#"，这个符号必须 not in s**（插入相同特殊符号并不会影响回文性质，且最终返回的最长回文子串一定以#开始、以#结束）  
  
  比如，原字符串 ```s = "google"```, 进行过插入特殊符号后，字符串由 t 表示：```t = "#g#o#o#g#l#e#"```, 这样处理后的字符串总长度总是奇数，因为插入 "#" 的个数等于原字符串长度加一。这就巧妙的避免了要分情况处理子串。
  
2). 计算半径数组p  
数组```p```指对于每个位置```i```，存在的最长半径```p[i]```，使得```t[i-p[i],i+p[i]]```是回文子串。  
  
若是贪婪算法，需要对每个```i```，初始化```p[i]=0```，然后递增```p[i]```直至找出以```i```为中心的最长回文子串```t[i-p[i],i+p[i]]```。  
  
 Manacher的精髓：使用已求出的位置```j```对应的最大半径```p[j]```，作为现在要求的位置```i```对应的```p[i]```的下界。
 
怎么计算```p[i]```呢? Manacher算法增加两个辅助变量: center和rear,表示已知的、以 center 为中心的最长回文子串的半径是rear, 且```rear = p[center]```.  
  
Manacher的精髓就在于此: 当```rear>i```时,```p[i]```有一个最小值```p[j]```.```if rear>i: p[i] = min(P[j],rear-i]),``` ```rear-i>0``` → ```rear>i``` → ```p[i] = max(0, min(rear-i, p[j]))```,其中```j = center - (i - center)```, j是i关于center的对称.  
从对称角度看,以```j```为中心、```p[j]```为半径的回文子串应当与以```i```为中心的字符相等,至少在半径```rear-i```内是相等的。于是，```p[j]```就相当于```p[i]```的一个下界。


```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        if len(s) < 2: return s
        t = "^#" + '#'.join(s) + '#$' #拓展s，使得每个字符的下标都是偶数
        center = rear = 0 #center回文子串的中心，rear 回文子串的结尾下标
        p = [0]*len(t) # p[i] 对于每个字符作为中心，扩展的最长回文子串的半径
        
        for i in range(1, len(t)-1):
            mirror = center - (i - center) # i 关于center的对称
            p[i] = max(0, min(rear-i, p[mirror])) # 重点
            while t[i + p[i] + 1] == t[i - p[i] - 1]: # 以i为中心拓展
                p[i] += 1
            if p[i] + i > rear: # 更新中心点和子串结尾下标
                center = i
                rear = p[i] + i
        k,i = max((p[i],i) for i in range(1, len(t)-1)) #获取回文子串最长半径
        return s[(i-k)//2:(i+k)//2]
```

**结果：**  
![](https://github.com/DaisyLoveU/Python-learning/blob/master/LeetCode/img/leetcode5_res_3.png)  
  
**分析：**  
时间复杂度：$O(n)$，从左到右遍历全部字符，构建数组p，利用已经求出的p[j]来求p[i]，时间复杂度呈线性。  
空间复杂度：$O(n)$，额外的数组p
