
给定一个二维网格和一个单词，找出该单词是否存在于网格中。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。
```
示例:

board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

给定 word = "ABCCED", 返回 true.
给定 word = "SEE", 返回 true.
给定 word = "ABCB", 返回 false.
```

思路一：回溯法。。。遍历board，找到开始开始位置，开始dfs，  
  
`backtrack： ORT， options， restraints， termination`


```python
class Solution:
    def exist(self, board, word: str) -> bool:
#         @pysnooper.snoop()
        def dfs(i,j,index):
            if index==len(word)-1: return True
            
            tmp,board[i][j] = board[i][j],'@'  # 巧妙标注已访问元素
            for k in range(4):
                x,y = i+dx[k],j+dy[k]
                if 0 <= x < m and 0 <= y < n and board[x][y] != '@' \
                and board[x][y] == word[index+1]:
                    if dfs(x,y,index+1): return True
            board[i][j] = tmp  # 还原作案现场
            return False
        
        dx = [-1,1,0,0]
        dy = [0,0,-1,1]
        m,n = len(board),len(board[0])
        
        for i in range(m):
            for j in range(n):
                if board[i][j] == word[0]:
                    if dfs(i,j,0): return True
        return False
```


```python
s = Solution()
board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]]
word = "ABCCED"
```


```python
s.exist(board, word)
```




    True


