
给定一个二维网格 board 和一个字典中的单词列表 words，找出所有同时在二维网格和字典中出现的单词。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。
```
示例:

输入: 
words = ["oath","pea","eat","rain"] and board =
[
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]

输出: ["eat","oath"]
说明:
你可以假设所有输入都由小写字母 a-z 组成。

提示:

你需要优化回溯算法以通过更大数据量的测试。你能否早点停止回溯？
如果当前单词不存在于所有单词的前缀中，则可以立即停止回溯。什么样的数据结构可以有效地执行这样的操作？散列表是否可行？为什么？ 前缀树如何？如果你想学习如何实现一个基本的前缀树，请先查看这个问题： 实现Trie（前缀树）。
```

思路一：暴力搜索，遍历board，每个元素上下左右搜索   pass  
思路二：使用提示的`Trie`进行回溯剪枝，直接使用dict模拟字典树  



```python
class Solution:
    def findWords(self, board, words):
        def dfs(i, j, curt, dyct):
            curt += board[i][j]
            dyct = dyct[board[i][j]]

            if '#' in dyct: 
                res.add(curt)

            tmp,board[i][j] =  board[i][j], '@'  # 巧妙标记是否访问过board[i][j]
            for k in range(4):
                x,y = i+dx[k], j+dy[k]
                if 0 <= x < m and 0 <= y < n \
                    and board[x][y] != '@' and board[x][y] in dyct:
                        dfs(x, y, curt, dyct)
            board[i][j] = tmp
            
        trie = {}
        for word in words:
            node = trie
            for letter in word:
                node = node.setdefault(letter, {})
            node['#'] = '#'
            
        dx = [-1, 1, 0, 0]
        dy = [0, 0, -1, 1]
        res = set()
        
        m,n = len(board),len(board[0])
        for i in range(m):
            for j in range(n):
                if board[i][j] in trie:
                    dfs(i, j, "", trie)
                    
        return list(res)
```

```在之前思路上 优化代码```


```python
class Solution:
    def findWords(self, board, words):
        if not board or not board[0] or not words: 
            return []
        m, n = len(board), len(board[0])
#         构建Trie
        root = {}
        for word in words:
            node = root
            for letter in word:
                node = node.setdefault(letter, {})
            node["#"] = word  # 以‘#’: word 结束，可以直接添加
#         进行上下左右搜索
        dx = [-1, 1, 0, 0]
        dy = [0, 0, -1, 1]
        
        def dfs(i, j, trie):
            if '#' in trie:
                res.add(trie['#'])
            tmp, board[i][j] = board[i][j], '@'  # ‘@’标注已访问元素
            for k in range(4):
                x,y = i+dx[k],j+dy[k]
                if 0 <= x < m and 0 <= y < n \
                    and board[x][y] != '@' and board[x][y] in trie:
                    dfs(x, y, trie[board[x][y]])
            board[i][j] = tmp  # 还原作案现场

        res = set()
        for i in range(m):
            for j in range(n): 
                if board[i][j] in root:
                    dfs(i, j, root[board[i][j]])
        return list(res)     
```


```python
s = Solution()
```


```python
res = s.findWords([
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']],["oath","pea","eat","rain"])
```


```python
res
```




    ['eat', 'oath']


