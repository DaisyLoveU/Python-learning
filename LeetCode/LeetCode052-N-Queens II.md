
和LeetCode051问题一样，只不过返回结果要求的是NQueens解的数量  
按照回溯思想的代码，只需修改返回结果即可


```python
class Solution:
    
    def totalNQueens(self, n):
        
        def dfs(curt, pie, na):
            row = len(curt)
            if row == n:
                res.append(curt)
            for col in range(n):
                if col not in curt and row+col not in pie and row-col not in na:
                    dfs(curt+[col], pie+[row+col], na+[row-col])
        res = []
        dfs([],[],[])
        return len(res)
```


```python
class Solution:    
    def totalNQueens(self, n: int) -> int:
        if n<1:return []
        self.res = []
        self.cols,self.pie,self.na = [],[],[]
        
        self.dfs(n, 0, [])
        return len(self.res)
    
    def dfs(self, n, row, curt):
        if row == n:
            self.res.append(curt)
            return
        
        for col in range(n):
            if col in self.cols or row+col in self.pie or row-col in self.na:
                continue
            self.cols.append(col)
            self.pie.append(row+col)
            self.na.append(row-col)
            
            self.dfs(n, row+1, curt+[col])
            
            self.cols.pop()
            self.pie.pop()
            self.na.pop()
```
