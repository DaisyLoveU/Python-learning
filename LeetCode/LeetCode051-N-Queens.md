
n 皇后问题研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。  

给定一个整数 n，返回所有不同的 n 皇后问题的解决方案。

每一种解法包含一个明确的 n 皇后问题的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。
```
示例:

输入: 4
输出: [
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
解释: 4 皇后问题存在两个不同的解法。
```

用三个set()记录矩阵内因放入皇后而封住的格子  
  
self.col是列，self.pie是row+col表示的 '/' 方向 self.na是row-col表示的 '\' 方向。  
  
参数cur_state的第i个元素表示第i行第cut_state[i]个位置放置皇后  
  

用深度优先搜索方法，逐行递归下去，递归终止条件是行数加到n时，此时即生成了一种解决方案，放入结果数组。  
  
每次递归内，迭代第row行第col列，检查这个点位是否安全，即row和col是否存在于三个set中，安全的话,就把自身攻击范围添加至三个set内，继续下个递归，cur_state.append这一行的列值 col。

递归函数后记得取出放置的皇后，即更新self.col,self.pie,self.na。




```python
class Solution:
    import pysnooper
#     @pysnooper.snoop()
    def solveNQ(self, n):
        if n < 1: return []
        self.res = []
        self.cols,self.pie,self.na = set(),set(),set()  # 使用集合来保存皇后攻击范围
        self.dfs(n, 0, [])
        return self.gen_res(n)
    
    @pysnooper.snoop()
    def dfs(self, n, row, cur_state):
        if row >= n:
            self.res.append(cur_state)
            return
        
        for col in range(n):
            if col in self.cols or col + row in self.pie or row-col in self.na:
                # 若第row行第col列可被攻击到，则退出此次循环，寻找下个可安置皇后的位置
                continue
            # 若第row行和col列是安全的，则可放置皇后，并添加该皇后的攻击范围
            self.cols.add(col)
            self.pie.add(row+col)
            self.na.add(row-col)
            
            self.dfs(n, row+1, cur_state+[col])
            # 继续找下一行可放置皇后的位置
            
            # 按照刚才放置皇后，若不能放置所有的皇后时，需要还原作案现场
            self.cols.remove(col)
            self.pie.remove(row+col)
            self.na.remove(row-col)
            
#     @pysnooper.snoop()        
    def gen_res(self, n):
        board = []
        for res in self.res:
            for i in res:
                board.append('.'*i + 'Q'+ '.'*(n-1-i))
        return [board[i:i+n] for i in range(0,len(board),n)]
```


```python
s = Solution()
```


```python
res = s.solveNQ(4)
print(res)
```

    [['.Q..', '...Q', 'Q...', '..Q.'], ['..Q.', 'Q...', '...Q', '.Q..']]
    

    Starting var:.. cur_state = []
    Starting var:.. n = 4
    Starting var:.. row = 0
    Starting var:.. self = <__main__.Solution object at 0x000001A12C18A8D0>
    15:30:59.155163 call        12     def dfs(self, n, row, cur_state):
    15:30:59.155163 line        13         if row >= n:
    15:30:59.155163 line        17         for col in range(n):
    New var:....... col = 0
    15:30:59.155163 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.155163 line        22             self.cols.add(col)
    15:30:59.155163 line        23             self.pie.add(row+col)
    15:30:59.156163 line        24             self.na.add(row-col)
    15:30:59.156163 line        26             self.dfs(n, row+1, cur_state+[col])
    Starting var:.. cur_state = [0]
    Starting var:.. n = 4
    Starting var:.. row = 1
    Starting var:.. self = <__main__.Solution object at 0x000001A12C18A8D0>
    15:30:59.156163 call        12     def dfs(self, n, row, cur_state):
    15:30:59.156163 line        13         if row >= n:
    15:30:59.156163 line        17         for col in range(n):
    New var:....... col = 0
    15:30:59.156163 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.156163 line        17         for col in range(n):
    Modified var:.. col = 1
    15:30:59.156163 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.156163 line        20                 continue
    15:30:59.157160 line        17         for col in range(n):
    Modified var:.. col = 2
    15:30:59.157160 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.157160 line        22             self.cols.add(col)
    15:30:59.157160 line        23             self.pie.add(row+col)
    15:30:59.157160 line        24             self.na.add(row-col)
    15:30:59.157160 line        26             self.dfs(n, row+1, cur_state+[col])
    Starting var:.. cur_state = [0, 2]
    Starting var:.. n = 4
    Starting var:.. row = 2
    Starting var:.. self = <__main__.Solution object at 0x000001A12C18A8D0>
    15:30:59.157160 call        12     def dfs(self, n, row, cur_state):
    15:30:59.157160 line        13         if row >= n:
    15:30:59.157160 line        17         for col in range(n):
    New var:....... col = 0
    15:30:59.157160 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.158160 line        17         for col in range(n):
    Modified var:.. col = 1
    15:30:59.158160 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.158160 line        17         for col in range(n):
    Modified var:.. col = 2
    15:30:59.158160 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.158160 line        17         for col in range(n):
    Modified var:.. col = 3
    15:30:59.158160 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.158160 line        20                 continue
    15:30:59.158160 line        17         for col in range(n):
    15:30:59.160161 return      17         for col in range(n):
    Return value:.. None
    15:30:59.160161 line        30             self.cols.remove(col)
    15:30:59.160161 line        31             self.pie.remove(row+col)
    15:30:59.161159 line        32             self.na.remove(row-col)
    15:30:59.161159 line        17         for col in range(n):
    Modified var:.. col = 3
    15:30:59.161159 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.161159 line        22             self.cols.add(col)
    15:30:59.161159 line        23             self.pie.add(row+col)
    15:30:59.161159 line        24             self.na.add(row-col)
    15:30:59.161159 line        26             self.dfs(n, row+1, cur_state+[col])
    Starting var:.. cur_state = [0, 3]
    Starting var:.. n = 4
    Starting var:.. row = 2
    Starting var:.. self = <__main__.Solution object at 0x000001A12C18A8D0>
    15:30:59.161159 call        12     def dfs(self, n, row, cur_state):
    15:30:59.162158 line        13         if row >= n:
    15:30:59.164157 line        17         for col in range(n):
    New var:....... col = 0
    15:30:59.164157 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.165156 line        17         for col in range(n):
    Modified var:.. col = 1
    15:30:59.165156 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.165156 line        22             self.cols.add(col)
    15:30:59.165156 line        23             self.pie.add(row+col)
    15:30:59.166156 line        24             self.na.add(row-col)
    15:30:59.166156 line        26             self.dfs(n, row+1, cur_state+[col])
    Starting var:.. cur_state = [0, 3, 1]
    Starting var:.. n = 4
    Starting var:.. row = 3
    Starting var:.. self = <__main__.Solution object at 0x000001A12C18A8D0>
    15:30:59.166156 call        12     def dfs(self, n, row, cur_state):
    15:30:59.166156 line        13         if row >= n:
    15:30:59.166156 line        17         for col in range(n):
    New var:....... col = 0
    15:30:59.166156 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.166156 line        17         for col in range(n):
    Modified var:.. col = 1
    15:30:59.166156 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.166156 line        17         for col in range(n):
    Modified var:.. col = 2
    15:30:59.166156 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.166156 line        20                 continue
    15:30:59.166156 line        17         for col in range(n):
    Modified var:.. col = 3
    15:30:59.166156 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.167155 line        17         for col in range(n):
    15:30:59.167155 return      17         for col in range(n):
    Return value:.. None
    15:30:59.167155 line        30             self.cols.remove(col)
    15:30:59.167155 line        31             self.pie.remove(row+col)
    15:30:59.167155 line        32             self.na.remove(row-col)
    15:30:59.167155 line        17         for col in range(n):
    Modified var:.. col = 2
    15:30:59.167155 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.167155 line        17         for col in range(n):
    Modified var:.. col = 3
    15:30:59.167155 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.167155 line        17         for col in range(n):
    15:30:59.167155 return      17         for col in range(n):
    Return value:.. None
    15:30:59.167155 line        30             self.cols.remove(col)
    15:30:59.167155 line        31             self.pie.remove(row+col)
    15:30:59.167155 line        32             self.na.remove(row-col)
    15:30:59.167155 line        17         for col in range(n):
    15:30:59.167155 return      17         for col in range(n):
    Return value:.. None
    15:30:59.167155 line        30             self.cols.remove(col)
    15:30:59.167155 line        31             self.pie.remove(row+col)
    15:30:59.167155 line        32             self.na.remove(row-col)
    15:30:59.167155 line        17         for col in range(n):
    Modified var:.. col = 1
    15:30:59.168154 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.168154 line        22             self.cols.add(col)
    15:30:59.168154 line        23             self.pie.add(row+col)
    15:30:59.168154 line        24             self.na.add(row-col)
    15:30:59.168154 line        26             self.dfs(n, row+1, cur_state+[col])
    Starting var:.. cur_state = [1]
    Starting var:.. n = 4
    Starting var:.. row = 1
    Starting var:.. self = <__main__.Solution object at 0x000001A12C18A8D0>
    15:30:59.168154 call        12     def dfs(self, n, row, cur_state):
    15:30:59.168154 line        13         if row >= n:
    15:30:59.168154 line        17         for col in range(n):
    New var:....... col = 0
    15:30:59.168154 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.168154 line        17         for col in range(n):
    Modified var:.. col = 1
    15:30:59.168154 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.168154 line        17         for col in range(n):
    Modified var:.. col = 2
    15:30:59.168154 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.168154 line        20                 continue
    15:30:59.169153 line        17         for col in range(n):
    Modified var:.. col = 3
    15:30:59.169153 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.169153 line        22             self.cols.add(col)
    15:30:59.169153 line        23             self.pie.add(row+col)
    15:30:59.169153 line        24             self.na.add(row-col)
    15:30:59.169153 line        26             self.dfs(n, row+1, cur_state+[col])
    Starting var:.. cur_state = [1, 3]
    Starting var:.. n = 4
    Starting var:.. row = 2
    Starting var:.. self = <__main__.Solution object at 0x000001A12C18A8D0>
    15:30:59.169153 call        12     def dfs(self, n, row, cur_state):
    15:30:59.169153 line        13         if row >= n:
    15:30:59.169153 line        17         for col in range(n):
    New var:....... col = 0
    15:30:59.169153 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.169153 line        22             self.cols.add(col)
    15:30:59.169153 line        23             self.pie.add(row+col)
    15:30:59.169153 line        24             self.na.add(row-col)
    15:30:59.169153 line        26             self.dfs(n, row+1, cur_state+[col])
    Starting var:.. cur_state = [1, 3, 0]
    Starting var:.. n = 4
    Starting var:.. row = 3
    Starting var:.. self = <__main__.Solution object at 0x000001A12C18A8D0>
    15:30:59.169153 call        12     def dfs(self, n, row, cur_state):
    15:30:59.169153 line        13         if row >= n:
    15:30:59.169153 line        17         for col in range(n):
    New var:....... col = 0
    15:30:59.169153 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.169153 line        17         for col in range(n):
    Modified var:.. col = 1
    15:30:59.170152 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.170152 line        17         for col in range(n):
    Modified var:.. col = 2
    15:30:59.170152 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.170152 line        22             self.cols.add(col)
    15:30:59.170152 line        23             self.pie.add(row+col)
    15:30:59.170152 line        24             self.na.add(row-col)
    15:30:59.170152 line        26             self.dfs(n, row+1, cur_state+[col])
    Starting var:.. cur_state = [1, 3, 0, 2]
    Starting var:.. n = 4
    Starting var:.. row = 4
    Starting var:.. self = <__main__.Solution object at 0x000001A12C18A8D0>
    15:30:59.170152 call        12     def dfs(self, n, row, cur_state):
    15:30:59.170152 line        13         if row >= n:
    15:30:59.171153 line        14             self.res.append(cur_state)
    15:30:59.171153 line        15             return
    15:30:59.171153 return      15             return
    Return value:.. None
    15:30:59.171153 line        30             self.cols.remove(col)
    15:30:59.171153 line        31             self.pie.remove(row+col)
    15:30:59.171153 line        32             self.na.remove(row-col)
    15:30:59.171153 line        17         for col in range(n):
    Modified var:.. col = 3
    15:30:59.171153 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.172152 line        17         for col in range(n):
    15:30:59.172152 return      17         for col in range(n):
    Return value:.. None
    15:30:59.173155 line        30             self.cols.remove(col)
    15:30:59.174151 line        31             self.pie.remove(row+col)
    15:30:59.174151 line        32             self.na.remove(row-col)
    15:30:59.174151 line        17         for col in range(n):
    Modified var:.. col = 1
    15:30:59.174151 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.174151 line        17         for col in range(n):
    Modified var:.. col = 2
    15:30:59.175157 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.175157 line        17         for col in range(n):
    Modified var:.. col = 3
    15:30:59.175157 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.175157 line        17         for col in range(n):
    15:30:59.175157 return      17         for col in range(n):
    Return value:.. None
    15:30:59.175157 line        30             self.cols.remove(col)
    15:30:59.175157 line        31             self.pie.remove(row+col)
    15:30:59.175157 line        32             self.na.remove(row-col)
    15:30:59.176155 line        17         for col in range(n):
    15:30:59.176155 return      17         for col in range(n):
    Return value:.. None
    15:30:59.176155 line        30             self.cols.remove(col)
    15:30:59.176155 line        31             self.pie.remove(row+col)
    15:30:59.177149 line        32             self.na.remove(row-col)
    15:30:59.177149 line        17         for col in range(n):
    Modified var:.. col = 2
    15:30:59.177149 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.177149 line        22             self.cols.add(col)
    15:30:59.177149 line        23             self.pie.add(row+col)
    15:30:59.177149 line        24             self.na.add(row-col)
    15:30:59.177149 line        26             self.dfs(n, row+1, cur_state+[col])
    Starting var:.. cur_state = [2]
    Starting var:.. n = 4
    Starting var:.. row = 1
    Starting var:.. self = <__main__.Solution object at 0x000001A12C18A8D0>
    15:30:59.178148 call        12     def dfs(self, n, row, cur_state):
    15:30:59.178148 line        13         if row >= n:
    15:30:59.178148 line        17         for col in range(n):
    New var:....... col = 0
    15:30:59.178148 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.178148 line        22             self.cols.add(col)
    15:30:59.178148 line        23             self.pie.add(row+col)
    15:30:59.178148 line        24             self.na.add(row-col)
    15:30:59.178148 line        26             self.dfs(n, row+1, cur_state+[col])
    Starting var:.. cur_state = [2, 0]
    Starting var:.. n = 4
    Starting var:.. row = 2
    Starting var:.. self = <__main__.Solution object at 0x000001A12C18A8D0>
    15:30:59.178148 call        12     def dfs(self, n, row, cur_state):
    15:30:59.178148 line        13         if row >= n:
    15:30:59.178148 line        17         for col in range(n):
    New var:....... col = 0
    15:30:59.178148 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.178148 line        17         for col in range(n):
    Modified var:.. col = 1
    15:30:59.178148 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.178148 line        20                 continue
    15:30:59.178148 line        17         for col in range(n):
    Modified var:.. col = 2
    15:30:59.178148 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.178148 line        17         for col in range(n):
    Modified var:.. col = 3
    15:30:59.179151 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.179151 line        22             self.cols.add(col)
    15:30:59.179151 line        23             self.pie.add(row+col)
    15:30:59.179151 line        24             self.na.add(row-col)
    15:30:59.179151 line        26             self.dfs(n, row+1, cur_state+[col])
    Starting var:.. cur_state = [2, 0, 3]
    Starting var:.. n = 4
    Starting var:.. row = 3
    Starting var:.. self = <__main__.Solution object at 0x000001A12C18A8D0>
    15:30:59.179151 call        12     def dfs(self, n, row, cur_state):
    15:30:59.179151 line        13         if row >= n:
    15:30:59.179151 line        17         for col in range(n):
    New var:....... col = 0
    15:30:59.179151 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.179151 line        17         for col in range(n):
    Modified var:.. col = 1
    15:30:59.179151 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.180147 line        22             self.cols.add(col)
    15:30:59.180147 line        23             self.pie.add(row+col)
    15:30:59.180147 line        24             self.na.add(row-col)
    15:30:59.180147 line        26             self.dfs(n, row+1, cur_state+[col])
    Starting var:.. cur_state = [2, 0, 3, 1]
    Starting var:.. n = 4
    Starting var:.. row = 4
    Starting var:.. self = <__main__.Solution object at 0x000001A12C18A8D0>
    15:30:59.180147 call        12     def dfs(self, n, row, cur_state):
    15:30:59.180147 line        13         if row >= n:
    15:30:59.180147 line        14             self.res.append(cur_state)
    15:30:59.180147 line        15             return
    15:30:59.180147 return      15             return
    Return value:.. None
    15:30:59.180147 line        30             self.cols.remove(col)
    15:30:59.180147 line        31             self.pie.remove(row+col)
    15:30:59.180147 line        32             self.na.remove(row-col)
    15:30:59.180147 line        17         for col in range(n):
    Modified var:.. col = 2
    15:30:59.180147 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.180147 line        17         for col in range(n):
    Modified var:.. col = 3
    15:30:59.180147 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.180147 line        17         for col in range(n):
    15:30:59.180147 return      17         for col in range(n):
    Return value:.. None
    15:30:59.180147 line        30             self.cols.remove(col)
    15:30:59.181146 line        31             self.pie.remove(row+col)
    15:30:59.181146 line        32             self.na.remove(row-col)
    15:30:59.181146 line        17         for col in range(n):
    15:30:59.181146 return      17         for col in range(n):
    Return value:.. None
    15:30:59.181146 line        30             self.cols.remove(col)
    15:30:59.181146 line        31             self.pie.remove(row+col)
    15:30:59.181146 line        32             self.na.remove(row-col)
    15:30:59.181146 line        17         for col in range(n):
    Modified var:.. col = 1
    15:30:59.181146 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.181146 line        17         for col in range(n):
    Modified var:.. col = 2
    15:30:59.181146 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.181146 line        17         for col in range(n):
    Modified var:.. col = 3
    15:30:59.181146 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.181146 line        20                 continue
    15:30:59.181146 line        17         for col in range(n):
    15:30:59.181146 return      17         for col in range(n):
    Return value:.. None
    15:30:59.181146 line        30             self.cols.remove(col)
    15:30:59.181146 line        31             self.pie.remove(row+col)
    15:30:59.181146 line        32             self.na.remove(row-col)
    15:30:59.181146 line        17         for col in range(n):
    Modified var:.. col = 3
    15:30:59.181146 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.181146 line        22             self.cols.add(col)
    15:30:59.182145 line        23             self.pie.add(row+col)
    15:30:59.182145 line        24             self.na.add(row-col)
    15:30:59.182145 line        26             self.dfs(n, row+1, cur_state+[col])
    Starting var:.. cur_state = [3]
    Starting var:.. n = 4
    Starting var:.. row = 1
    Starting var:.. self = <__main__.Solution object at 0x000001A12C18A8D0>
    15:30:59.182145 call        12     def dfs(self, n, row, cur_state):
    15:30:59.182145 line        13         if row >= n:
    15:30:59.182145 line        17         for col in range(n):
    New var:....... col = 0
    15:30:59.182145 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.182145 line        22             self.cols.add(col)
    15:30:59.182145 line        23             self.pie.add(row+col)
    15:30:59.182145 line        24             self.na.add(row-col)
    15:30:59.182145 line        26             self.dfs(n, row+1, cur_state+[col])
    Starting var:.. cur_state = [3, 0]
    Starting var:.. n = 4
    Starting var:.. row = 2
    Starting var:.. self = <__main__.Solution object at 0x000001A12C18A8D0>
    15:30:59.182145 call        12     def dfs(self, n, row, cur_state):
    15:30:59.182145 line        13         if row >= n:
    15:30:59.182145 line        17         for col in range(n):
    New var:....... col = 0
    15:30:59.182145 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.182145 line        17         for col in range(n):
    Modified var:.. col = 1
    15:30:59.182145 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.182145 line        17         for col in range(n):
    Modified var:.. col = 2
    15:30:59.182145 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.183145 line        22             self.cols.add(col)
    15:30:59.183145 line        23             self.pie.add(row+col)
    15:30:59.183145 line        24             self.na.add(row-col)
    15:30:59.183145 line        26             self.dfs(n, row+1, cur_state+[col])
    Starting var:.. cur_state = [3, 0, 2]
    Starting var:.. n = 4
    Starting var:.. row = 3
    Starting var:.. self = <__main__.Solution object at 0x000001A12C18A8D0>
    15:30:59.183145 call        12     def dfs(self, n, row, cur_state):
    15:30:59.183145 line        13         if row >= n:
    15:30:59.183145 line        17         for col in range(n):
    New var:....... col = 0
    15:30:59.183145 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.183145 line        17         for col in range(n):
    Modified var:.. col = 1
    15:30:59.183145 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.183145 line        17         for col in range(n):
    Modified var:.. col = 2
    15:30:59.183145 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.183145 line        17         for col in range(n):
    Modified var:.. col = 3
    15:30:59.183145 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.183145 line        17         for col in range(n):
    15:30:59.183145 return      17         for col in range(n):
    Return value:.. None
    15:30:59.183145 line        30             self.cols.remove(col)
    15:30:59.183145 line        31             self.pie.remove(row+col)
    15:30:59.183145 line        32             self.na.remove(row-col)
    15:30:59.184143 line        17         for col in range(n):
    Modified var:.. col = 3
    15:30:59.184143 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.184143 line        17         for col in range(n):
    15:30:59.184143 return      17         for col in range(n):
    Return value:.. None
    15:30:59.184143 line        30             self.cols.remove(col)
    15:30:59.184143 line        31             self.pie.remove(row+col)
    15:30:59.184143 line        32             self.na.remove(row-col)
    15:30:59.184143 line        17         for col in range(n):
    Modified var:.. col = 1
    15:30:59.184143 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.184143 line        22             self.cols.add(col)
    15:30:59.184143 line        23             self.pie.add(row+col)
    15:30:59.185146 line        24             self.na.add(row-col)
    15:30:59.185146 line        26             self.dfs(n, row+1, cur_state+[col])
    Starting var:.. cur_state = [3, 1]
    Starting var:.. n = 4
    Starting var:.. row = 2
    Starting var:.. self = <__main__.Solution object at 0x000001A12C18A8D0>
    15:30:59.185146 call        12     def dfs(self, n, row, cur_state):
    15:30:59.185146 line        13         if row >= n:
    15:30:59.185146 line        17         for col in range(n):
    New var:....... col = 0
    15:30:59.185146 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.185146 line        17         for col in range(n):
    Modified var:.. col = 1
    15:30:59.185146 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.185146 line        17         for col in range(n):
    Modified var:.. col = 2
    15:30:59.185146 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.185146 line        20                 continue
    15:30:59.185146 line        17         for col in range(n):
    Modified var:.. col = 3
    15:30:59.186143 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.186143 line        17         for col in range(n):
    15:30:59.186143 return      17         for col in range(n):
    Return value:.. None
    15:30:59.186143 line        30             self.cols.remove(col)
    15:30:59.186143 line        31             self.pie.remove(row+col)
    15:30:59.186143 line        32             self.na.remove(row-col)
    15:30:59.186143 line        17         for col in range(n):
    Modified var:.. col = 2
    15:30:59.186143 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.186143 line        17         for col in range(n):
    Modified var:.. col = 3
    15:30:59.186143 line        18             if col in self.cols or col + row in self.pie or row-col in self.na:
    15:30:59.186143 line        17         for col in range(n):
    15:30:59.186143 return      17         for col in range(n):
    Return value:.. None
    15:30:59.186143 line        30             self.cols.remove(col)
    15:30:59.186143 line        31             self.pie.remove(row+col)
    15:30:59.186143 line        32             self.na.remove(row-col)
    15:30:59.187142 line        17         for col in range(n):
    15:30:59.187142 return      17         for col in range(n):
    Return value:.. None
    

```分析：cur_state[i]存放的是第i行的皇后所在的位置，递归以行的形式递归，每次放置的皇后要判断是否与前f面已经放置的皇后冲突。从cur_state[row] = 0开始一直到n-1，判断是否安全 如果安全就进行下一行的摆放，每次递归到row==n的时候表示当前所有n个皇后已经摆放完成，此时将当前完成的结果保存在res，后根据cur_state[i]存放i行皇后的位置的特性将res数组里面res[i][cur_state[i]]置为’Q’,return。这样递归结束就能找到所有的摆放方法。这是一个深度优先的过程，从在第一行放在第一个位置开始，摆放第二行、第三行…直到最后一行。然后cur_state[row]++,表示将第一行放在第二个位置…然后摆放第二行、第三行…直到最后一行……直到所有的情况深度优先搜索完成```

更简洁的代码


```python
class Solution:
    def solveNQueens(self, n):
        import pysnooper
        @pysnooper.snoop()
        def dfs(curt, pie, na):
            '''
            curt:一维列表第i个元素代表棋盘第i行第curt[i]个位置放着皇后,同时表示第curt[i]列不安全
            pie: row+col，表示/方向不安全
            na:  row-col, 表示\方向不安全
            '''
            row = len(curt) 
            if row == n:
                res.append(curt)
                return
            
            for col in range(n):
                if col not in curt and row+col not in pie and row-col not in na:
#                     如果当前位置安全,则放置皇后,添加皇后的攻击范围
                    dfs(curt+[col], pie+[row+col], na+[row-col])
#                     这里不能用curt.append(col)，因为list.append没有返回值，也就是说返回的None，会导致下面调用出错
                    
        res = []
        dfs([], [], [])
        return [['.'*i + 'Q' + '.'*(n-i-1) for i in j] for j in res]
```
