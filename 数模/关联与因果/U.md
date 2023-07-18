```python
from scipy import stats
import matplotlib.pyplot as plt
x= np.array([3,5,1,6,7,2,8,8,4])
y = np.array([5,3,2,6,8,1,7,8,4])
plt.plot(x,y,label='suiji')
result  = stats.pearsonr( x ,y )
print( "pearsonr",result )
result  =stats.spearmanr( x ,y )
print( "spearmanr",result )
result  =stats.kendalltau( x ,y )
print( "kendalltau",result )
```


