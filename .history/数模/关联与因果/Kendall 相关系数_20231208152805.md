
肯德尔秩相关 $\tau$
肯德尔相关也是一种秩相关系数，是基于数据对象的秩来进行两个随机变量之间的相关关系，所分析的目标对象应该是一种有序的类别变量

与斯皮尔曼相关不同的是，斯皮尔曼是基于秩差而肯德尔是基于样本数据对之间的关系来进行相关性强弱的分析

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230329190743.png)

## 两个公式

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230329190839.png)

```python
# Example4 -- Kendall correlation coefficient
from scipy.stats import kendalltau
import matplotlib.pyplot as plt
dat1 = np.array([3,5,1,9,7,2,8,4,6])
dat2 = np.array([5,3,2,6,8,1,7,9,4])
c = 0
d = 0
for i in range(len(dat1)):
    for j in range(i+1,len(dat1)):
        if (dat1[i]-dat1[j])*(dat2[i]-dat2[j])>0:
            c = c + 1
        else:
            d = d + 1
k_tau = (c - d) * 2 / len(dat1)/(len(dat1)-1)
            
print('k_tau = {0}'.format(k_tau))    
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230329191224.png)

```python
# Tau_b
from scipy.stats import kendalltau
 
dat1 = np.array([3,5,1,6,7,2,8,8,4])
dat2 = np.array([5,3,2,6,8,1,7,8,4])
#dat1 = np.array([3,5,1,9,7,2,8,4,6])
#dat2 = np.array([5,3,2,6,8,1,7,9,4])
c = 0
d = 0
t_x = 0
t_y = 0
for i in range(len(dat1)):
    for j in range(i+1,len(dat1)):
        if (dat1[i]-dat1[j])*(dat2[i]-dat2[j])>0:
            c = c + 1
        elif (dat1[i]-dat1[j])*(dat2[i]-dat2[j])<0:
            d = d + 1
        else:
            if (dat1[i]-dat1[j])==0 and (dat2[i]-dat2[j])!=0:
                t_x = t_x + 1
            elif (dat1[i]-dat1[j])!=0 and (dat2[i]-dat2[j])==0:
                t_y = t_y + 1
                
tau_b = (c - d) / np.sqrt((c+d+t_x)*(c+d+t_y))
            
print('tau_b = {0}'.format(tau_b))            
print('kendalltau(dat1,dat2) =  {0}'.format(kendalltau(dat1,dat2)))
```
