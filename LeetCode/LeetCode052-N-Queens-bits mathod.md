
位运算 巧用之 N-Queens


```python
class Solution:
    
    def totalNQueens(self, n):
        if n < 1: return []
        self.cnt = 0
        self.dfs(n, 0, 0, 0, 0)
#         位运算，0表示安全，1表示可受到攻击
        return self.cnt
    
    def dfs(self, n, row, cols, pie, na):
#         recursion terminator
        if row >= n:
            self.cnt += 1
            return
        status = (~(cols | pie | na) & (1 << n) -1)  # 通过位运算得到当前的空位
        # cols | pie | na 通过 or 操作得到目前所有列的状态， 再取反，用 1 表示安全的列
        # (1 << n) -1) 得到低 n 位全 1 ,高位为 0 ,  and 操作过滤掉高位 只取低 n 位  
        
        while status:
            p = status & -status  # 位运算得到最低位的1  即当前行可放皇后的位置
            self.dfs(n, row+1, cols|p, (pie|p)<<1, (na|p)>>1)
            # 进行下一层 dfs, cols|p 更新列状态
            # (pie|p)<<1 和 (na|p)>>1 表示下一行中受到皇后攻击的位置
            status = status & (status-1)  # 去掉最低位的1 即表示在 p 位置放置皇后，去掉一个安全的列
```

读懂骚操作 和思想
