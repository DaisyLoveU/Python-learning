
给出 n 代表生成括号的对数，请你写出一个函数，使其能够生成所有可能的并且有效的括号组合。
```
例如，给出 n = 3，生成结果为：

[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```
思路： 回溯法  


非常牛逼的讲解，需要这样的人来给我们讲算法

#### 以Generate Parentheses为例，backtrack的题到底该怎么去思考？


所谓Backtracking都是这样的思路：在当前局面下，你有若干种选择。那么尝试每一种选择。如果已经发现某种选择肯定不行（因为违反了某些限定条件），就返回；如果某种选择试到最后发现是正确解，就将其加入解集

所以你思考递归题时，只要明确三点就行：选择 (Options)，限制 (Restraints)，结束条件 (Termination)。即“ORT原则”（这个是我自己编的）




对于这道题，在任何时刻，你都有两种选择：
1. 加左括号。
2. 加右括号。

同时有以下限制：
1. 如果左括号已经用完了，则不能再加左括号了。
2. 如果已经出现的右括号和左括号一样多，则不能再加右括号了。因为那样的话新加入的右括号一定无法匹配。

结束条件是：
左右括号都已经用完。

结束后的正确性：
左右括号用完以后，一定是正确解。因为1. 左右括号一样多，2. 每个右括号都一定有与之配对的左括号。因此一旦结束就可以加入解集（有时也可能出现结束以后不一定是正确解的情况，这时要多一步判断）。

递归函数传入参数：
限制和结束条件中有“用完”和“一样多”字样，因此你需要知道左右括号的数目。
当然你还需要知道当前局面sublist和解集res。

因此，把上面的思路拼起来就是代码：

	if (左右括号都已用完) {
	  加入解集，返回
	}
	//否则开始试各种选择
	if (还有左括号可以用) {
	  加一个左括号，继续递归
	}
	if (右括号小于左括号) {
	  加一个右括号，继续递归
	}
	
	
	
你帖的那段代码逻辑中加了一条限制：“3. 是否还有右括号剩余。如有才加右括号”。这是合理的。不过对于这道题，如果满足限制1、2时，3一定自动满足，所以可以不判断3。

这题其实是最好的backtracking初学练习之一，因为ORT三者都非常简单明显。你不妨按上述思路再梳理一遍，还有问题的话再说。



以上文字来自 1point3arces的牛人解答  


复杂度分析

我们的复杂度分析依赖于理解 generateParenthesis(n) 中有多少个元素。这个分析超出了本文的范畴，但事实证明这是第 n 个卡塔兰数.  

时间复杂度：O(4^n/sqrt(n))，在回溯过程中，每个有效序列最多需要 n 步。

空间复杂度：O(4^n/sqrt(n))，并使用O(n) 的空间来存储序列。


```python
class Solution:
    def generateParenthesis(self, n):
        # if n == 0: return ''
        self.res = []
        self.gen('', n, n)
        return self.res
    import pysnooper
    @pysnooper.snoop()
    def gen(self, sub, left, right):
        if left == 0 and right == 0:
            self.res.append(sub)
            return
        
        if left > 0:
            self.gen(sub+'(', left-1, right)
        if right > left:
            self.gen(sub+')', left, right-1)
```


```python
s = Solution()
```


```python
s.generateParenthesis(3)
```

    Starting var:.. left = 3
    Starting var:.. right = 3
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = ''
    21:24:19.191288 call         9     def gen(self, sub, left, right):
    21:24:19.193285 line        10         if left == 0 and right == 0:
    21:24:19.193285 line        14         if left > 0:
    21:24:19.194286 line        15             self.gen(sub+'(', left-1, right)
    Starting var:.. left = 2
    Starting var:.. right = 3
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '('
    21:24:19.194286 call         9     def gen(self, sub, left, right):
    21:24:19.194286 line        10         if left == 0 and right == 0:
    21:24:19.196285 line        14         if left > 0:
    21:24:19.196285 line        15             self.gen(sub+'(', left-1, right)
    Starting var:.. left = 1
    Starting var:.. right = 3
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '(('
    21:24:19.197284 call         9     def gen(self, sub, left, right):
    21:24:19.197284 line        10         if left == 0 and right == 0:
    21:24:19.197284 line        14         if left > 0:
    21:24:19.197284 line        15             self.gen(sub+'(', left-1, right)
    Starting var:.. left = 0
    Starting var:.. right = 3
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '((('
    21:24:19.202280 call         9     def gen(self, sub, left, right):
    21:24:19.202280 line        10         if left == 0 and right == 0:
    21:24:19.203282 line        14         if left > 0:
    21:24:19.203282 line        16         if right > left:
    21:24:19.203282 line        17             self.gen(sub+')', left, right-1)
    Starting var:.. left = 0
    Starting var:.. right = 2
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '((()'
    21:24:19.204280 call         9     def gen(self, sub, left, right):
    21:24:19.204280 line        10         if left == 0 and right == 0:
    21:24:19.204280 line        14         if left > 0:
    21:24:19.204280 line        16         if right > left:
    21:24:19.204280 line        17             self.gen(sub+')', left, right-1)
    Starting var:.. left = 0
    Starting var:.. right = 1
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '((())'
    21:24:19.204280 call         9     def gen(self, sub, left, right):
    21:24:19.205280 line        10         if left == 0 and right == 0:
    21:24:19.205280 line        14         if left > 0:
    21:24:19.205280 line        16         if right > left:
    21:24:19.205280 line        17             self.gen(sub+')', left, right-1)
    Starting var:.. left = 0
    Starting var:.. right = 0
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '((()))'
    21:24:19.205280 call         9     def gen(self, sub, left, right):
    21:24:19.205280 line        10         if left == 0 and right == 0:
    21:24:19.205280 line        11             self.res.append(sub)
    21:24:19.205280 line        12             return
    21:24:19.205280 return      12             return
    Return value:.. None
    21:24:19.205280 return      17             self.gen(sub+')', left, right-1)
    Return value:.. None
    21:24:19.206278 return      17             self.gen(sub+')', left, right-1)
    Return value:.. None
    21:24:19.206278 return      17             self.gen(sub+')', left, right-1)
    Return value:.. None
    21:24:19.207278 line        16         if right > left:
    21:24:19.207278 line        17             self.gen(sub+')', left, right-1)
    Starting var:.. left = 1
    Starting var:.. right = 2
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '(()'
    21:24:19.207278 call         9     def gen(self, sub, left, right):
    21:24:19.207278 line        10         if left == 0 and right == 0:
    21:24:19.207278 line        14         if left > 0:
    21:24:19.208277 line        15             self.gen(sub+'(', left-1, right)
    Starting var:.. left = 0
    Starting var:.. right = 2
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '(()('
    21:24:19.208277 call         9     def gen(self, sub, left, right):
    21:24:19.209282 line        10         if left == 0 and right == 0:
    21:24:19.209282 line        14         if left > 0:
    21:24:19.209282 line        16         if right > left:
    21:24:19.209282 line        17             self.gen(sub+')', left, right-1)
    Starting var:.. left = 0
    Starting var:.. right = 1
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '(()()'
    21:24:19.209282 call         9     def gen(self, sub, left, right):
    21:24:19.209282 line        10         if left == 0 and right == 0:
    21:24:19.210275 line        14         if left > 0:
    21:24:19.210275 line        16         if right > left:
    21:24:19.210275 line        17             self.gen(sub+')', left, right-1)
    Starting var:.. left = 0
    Starting var:.. right = 0
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '(()())'
    21:24:19.210275 call         9     def gen(self, sub, left, right):
    21:24:19.210275 line        10         if left == 0 and right == 0:
    21:24:19.210275 line        11             self.res.append(sub)
    21:24:19.210275 line        12             return
    21:24:19.210275 return      12             return
    Return value:.. None
    21:24:19.210275 return      17             self.gen(sub+')', left, right-1)
    Return value:.. None
    21:24:19.211275 return      17             self.gen(sub+')', left, right-1)
    Return value:.. None
    21:24:19.211275 line        16         if right > left:
    21:24:19.211275 line        17             self.gen(sub+')', left, right-1)
    Starting var:.. left = 1
    Starting var:.. right = 1
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '(())'
    21:24:19.211275 call         9     def gen(self, sub, left, right):
    21:24:19.211275 line        10         if left == 0 and right == 0:
    21:24:19.211275 line        14         if left > 0:
    21:24:19.211275 line        15             self.gen(sub+'(', left-1, right)
    Starting var:.. left = 0
    Starting var:.. right = 1
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '(())('
    21:24:19.212275 call         9     def gen(self, sub, left, right):
    21:24:19.212275 line        10         if left == 0 and right == 0:
    21:24:19.212275 line        14         if left > 0:
    21:24:19.212275 line        16         if right > left:
    21:24:19.212275 line        17             self.gen(sub+')', left, right-1)
    Starting var:.. left = 0
    Starting var:.. right = 0
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '(())()'
    21:24:19.212275 call         9     def gen(self, sub, left, right):
    21:24:19.212275 line        10         if left == 0 and right == 0:
    21:24:19.214273 line        11             self.res.append(sub)
    21:24:19.214273 line        12             return
    21:24:19.214273 return      12             return
    Return value:.. None
    21:24:19.215273 return      17             self.gen(sub+')', left, right-1)
    Return value:.. None
    21:24:19.215273 line        16         if right > left:
    21:24:19.215273 return      16         if right > left:
    Return value:.. None
    21:24:19.215273 return      17             self.gen(sub+')', left, right-1)
    Return value:.. None
    21:24:19.215273 return      17             self.gen(sub+')', left, right-1)
    Return value:.. None
    21:24:19.216273 line        16         if right > left:
    21:24:19.216273 line        17             self.gen(sub+')', left, right-1)
    Starting var:.. left = 2
    Starting var:.. right = 2
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '()'
    21:24:19.216273 call         9     def gen(self, sub, left, right):
    21:24:19.216273 line        10         if left == 0 and right == 0:
    21:24:19.217273 line        14         if left > 0:
    21:24:19.217273 line        15             self.gen(sub+'(', left-1, right)
    Starting var:.. left = 1
    Starting var:.. right = 2
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '()('
    21:24:19.217273 call         9     def gen(self, sub, left, right):
    21:24:19.217273 line        10         if left == 0 and right == 0:
    21:24:19.218271 line        14         if left > 0:
    21:24:19.218271 line        15             self.gen(sub+'(', left-1, right)
    Starting var:.. left = 0
    Starting var:.. right = 2
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '()(('
    21:24:19.227267 call         9     def gen(self, sub, left, right):
    21:24:19.227267 line        10         if left == 0 and right == 0:
    21:24:19.228265 line        14         if left > 0:
    21:24:19.228265 line        16         if right > left:
    21:24:19.228265 line        17             self.gen(sub+')', left, right-1)
    Starting var:.. left = 0
    Starting var:.. right = 1
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '()(()'
    21:24:19.228265 call         9     def gen(self, sub, left, right):
    21:24:19.228265 line        10         if left == 0 and right == 0:
    21:24:19.228265 line        14         if left > 0:
    21:24:19.229264 line        16         if right > left:
    21:24:19.229264 line        17             self.gen(sub+')', left, right-1)
    Starting var:.. left = 0
    Starting var:.. right = 0
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '()(())'
    21:24:19.229264 call         9     def gen(self, sub, left, right):
    21:24:19.229264 line        10         if left == 0 and right == 0:
    21:24:19.229264 line        11             self.res.append(sub)
    21:24:19.229264 line        12             return
    21:24:19.230264 return      12             return
    Return value:.. None
    21:24:19.232263 return      17             self.gen(sub+')', left, right-1)
    Return value:.. None
    21:24:19.233262 return      17             self.gen(sub+')', left, right-1)
    Return value:.. None
    21:24:19.233262 line        16         if right > left:
    21:24:19.233262 line        17             self.gen(sub+')', left, right-1)
    Starting var:.. left = 1
    Starting var:.. right = 1
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '()()'
    21:24:19.233262 call         9     def gen(self, sub, left, right):
    21:24:19.235261 line        10         if left == 0 and right == 0:
    21:24:19.235261 line        14         if left > 0:
    21:24:19.235261 line        15             self.gen(sub+'(', left-1, right)
    Starting var:.. left = 0
    Starting var:.. right = 1
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '()()('
    21:24:19.235261 call         9     def gen(self, sub, left, right):
    21:24:19.235261 line        10         if left == 0 and right == 0:
    21:24:19.235261 line        14         if left > 0:
    21:24:19.235261 line        16         if right > left:
    21:24:19.235261 line        17             self.gen(sub+')', left, right-1)
    Starting var:.. left = 0
    Starting var:.. right = 0
    Starting var:.. self = <__main__.Solution object at 0x0000014A63D8D668>
    Starting var:.. sub = '()()()'
    21:24:19.236261 call         9     def gen(self, sub, left, right):
    21:24:19.236261 line        10         if left == 0 and right == 0:
    21:24:19.236261 line        11             self.res.append(sub)
    21:24:19.237260 line        12             return
    21:24:19.237260 return      12             return
    Return value:.. None
    21:24:19.237260 return      17             self.gen(sub+')', left, right-1)
    Return value:.. None
    21:24:19.237260 line        16         if right > left:
    21:24:19.237260 return      16         if right > left:
    Return value:.. None
    21:24:19.237260 return      17             self.gen(sub+')', left, right-1)
    Return value:.. None
    21:24:19.238259 line        16         if right > left:
    21:24:19.238259 return      16         if right > left:
    Return value:.. None
    21:24:19.238259 return      17             self.gen(sub+')', left, right-1)
    Return value:.. None
    21:24:19.238259 line        16         if right > left:
    21:24:19.238259 return      16         if right > left:
    Return value:.. None
    




    ['((()))', '(()())', '(())()', '()(())', '()()()']


