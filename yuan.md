初步打算：使用z3-solver求解一些数学问题，然后自己写代码去解决同样问题，在自己代码基础上使用pysnooper去调试，逐步打印出变量的值，并把一些解利用mongoDB保存下来


## 解线性方程
```python
from z3 import *

x, y = Reals('x y')
solve(x-y == 3, 3*x-8*y == 4)
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240130173711.png)

## 
```python
from z3 import *

x, y = Reals('x y')
solve(x>10,x-y == 3)

#[y = 8, x = 11]
```

## 八皇后问题

```python
from z3 import *
# 每个皇后必须在不同的行中，记录每行对应的皇后对应的列位置
Q = [Int(f'Q_{i}') for i in range(8)]

# 每个皇后在列 0,1,2,...,7
val_c = [And(0 <= Q[i], Q[i] <= 7) for i in range(8)]

# 每列最多一个皇后
col_c = [Distinct(Q)]

# 对角线约束
diag_c = [If(i == j,
             True,
             And(Q[i] - Q[j] != i - j, Q[i] - Q[j] != j - i))
          for i in range(8) for j in range(i)]
def print_eight_queen(result):
    for column in result:
        for i in range(8):
            if i == column:
                print(end="Q  ")
            else:
                print(end="*  ")
        print()


s = Solver()
s.add(val_c + col_c + diag_c)
if s.check() == sat:
    result = s.model()
    result = [result[Q[i]].as_long() for i in range(8)]
    print("每行皇后所在的列位置：", result)
    print_eight_queen(result)
```
 
 ![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240130144915.png)

