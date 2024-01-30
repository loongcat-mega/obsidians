高性能科学计算和数据分析的基础包

## ndarray

```python
a=[]
n=np.array(a)# 将列表转化为数组

array_empty=np.empty([10,10])# 10*10空矩阵
array_zero=np.zeros([10,10]) # 10*10浮点0矩阵
array_ones=np.ones([10,10])# 10*10浮点1矩阵

```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312111025978.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312111027978.png)

## 创建随机数组

```python
np.random.rand(10,10) # 10*10 0-1随机数矩阵
np.random.uniform(0,100) # 指定范围内一个数
np.random.randint(0,100) #指定范围内一个整数

np.random.normal(1.75,0.1,(2,3)) #给定均值 标准差 维度的正态分布
```

## 查看数组属性

```python
b.size 数组元素个数
b.shape 数组形状
b.ndim 数组维度
b.dtype 数组元素类型
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312110957679.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312111024417.png)

## 矢量化

不用编写循环即可对数据执行批量运算

```python
a=[]
arr=np.array(a)
1/arr

arr-arr

arr*arr

arr**0.5
```

## 索引与切片

选取数据子集或者单个元素
数组切片是原始数组的视图，这意味着数据不会被复制，视图上的任何修改都会直接反映到源数组上
将一个标量值赋值给一个切片时，该值会自动传播到整个选区

```python
slice=arr[0:5] # 0 1 2 3 4
slice[:]=10# arr切片选区全为10
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312111007198.png)

## 数学和统计方法

```python
arr.sum()
arr.mean()
arr.std()


```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312111019880.png)

可接受一个axis参数 用于计算该轴向上得统计值，最终结果是一个一维的数组
```python
arr.mean(axis=1) # 计算横向轴向上的统计值
arr.sum(0)
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312111021997.png)

## 线性代数

矩阵乘法 矩阵分解 行列式

```python
x.dot(y) # 矩阵x点成矩阵y
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312111023853.png)

## 数组操作

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312111030523.png)
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312111030355.png)
