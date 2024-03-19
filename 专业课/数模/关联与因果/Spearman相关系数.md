也叫秩相关系数，又称等级相关系数，反映的是两个随机变量的变化趋势方向和强度之间的关联，是两个随机变量的样本值按数据的大小顺序排列位次，与各要素样本值的位次代替实际数据而求得的一种统计量

两个随机变量X，Y，如果秩相关系数为正，则Y随X增大而增大，若秩相关系数为负，则Y随X增大而减小

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230329185200.png)



![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230329183033.png)

斯皮尔曼相关系数>0为正相关，<0为负相关。越接近$\pm 1$ 相关性越强。
被定义成等级变量之间的皮尔逊相关系数

```python
import numpy as np
from scipy import stats
 
stats.spearmanr([3,5,1,6,7,2,8,9,4], [5,3,2,6,8,1,7,9,4])
```

```python
mport pandas as pd
import numpy as np
  
#原始数据
X1=pd.Series([1, 2, 3, 4, 5, 6])
Y1=pd.Series([0.3, 0.9, 2.7, 2, 3.5, 5])
  

r=X1.corr(Y1,method='spearman')
print(r,p) #0.942857142857143 0.9428571428571428
```

斯皮尔曼相关系数的假设检验分为大样本和小样本

### 小样本情况 n<30

直接查临界值表，样本相关系数r必须大于表中的临界值，才能得出显著的结论


### 大样本情况

构造统计量$r_s\sqrt {n-1}$服从标准正态$N(0,1)$ 

