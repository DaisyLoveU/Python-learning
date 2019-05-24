

```python
def zeroOnePack(N, V, weight, value):
    '''
    0-1 背包问题，每件物品只能取0个或1个
    f(i, j) = max{f(i-1, j), f(i-1, j-w[i]) + v[i]}
    :param N：物品个数
    :param V: 背包总容量
    :param weigt: 每个物品的容量，如 [5, 4, 7, 2, 6]
    :param value: 每个物品的价值，如 [12, 3, 10, 3, 6]
    :return: 背包最大价值
    '''
    f = [[0 for _ in range(V+1)] for _ in range(N+1)]
#     初始化价值矩阵
    for i in range(1, N+1):
        for j in range(1, V+1):
            if j < weight[i-1]:
                f[i][j] = f[i-1][j]
            else:
                f[i][j] = max(f[i-1][j], f[i-1][j-weight[i-1]] + value[i-1])
    return f[N][V]
```


```python
N = 5
V = 15
weight = [5, 4, 7, 2, 6]
value = [12, 3, 10, 3, 6]

zeroOnePack(N, V, weight, value)
```




    25




```python
def completePack(N, V, weight, value):
    '''
    完全背包问题，每个物品可以取无限次
    f(i, j) = max(f[i-1][j-k*weight[i-1]] + k*value[i-1], 0 <= k <= j//weight[i])
    
    '''
    f = [[0 for _ in range(V+1)] for _ in range(N+1)]
    for i in range(1, N+1):
        for j in range(1, V+1):
            f[i][j]= max([f[i-1][j-k*weight[i-1]] + k*value[i-1] for k in range(j//weight[i-1]+1)])
    return f[N][V]
```


```python
completePack(N, V, weight, value)
```




    36


