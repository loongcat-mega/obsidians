
# 图像增强

## 对比度

### 直方图

- 图像直方图是反映图像像素分布的统计表。 灰度直方图是图像灰度级的函数，用来描述每个灰度级在图像矩阵中的像素个数。
- 直方图均衡和直方图匹配都是基于整幅图像的灰度分布进行全局变换，并非针对图像局部区域的细节进行增强。
- 直方图处理对于局部同样适用，局部直方图处理的思想是基于像素邻域的灰度分布进行直方图变换处理。


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241223163701.png)

**若一副图像的灰度级分布均匀，则该图像会有高对比度的外观并且展示灰色色调的较大变化**

### 直方图均衡

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241223163823.png)

对原直方图做一个映射，使得映射后的直方图的概率密度分布函数为均匀分布

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241223164117.png)

均衡化之前：
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241223164152.png)
均衡化之后：
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241223164216.png)

均衡化前后直方图对比：
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241223164323.png)


###  直方图匹配（规定化）

**希望处理后的图像具有某种制定的直方图形状**，也就是使得输入图像具有指定的图像色彩风格预设

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241223164528.png)

简单来说，r和z均衡化后为s


### 局部直方图均衡

局部均衡化：
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241223173623.png)

原图，全局均衡化，局部均衡化直方图：
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241223173657.png)

局部均衡化是在原直方图基础上削峰填谷


## 平滑空间滤波器

平滑滤波器用于模糊处理和降低噪声

### 平滑线性滤波器

取邻域像素均值
通常是降低了图像灰度的“尖锐”变化，因此平滑处理应用就是降低噪声
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241223195624.png)

### 统计排序（非线性）滤波器

统计排序滤波器是一种非线性空间滤波器，这种滤波器的响应以滤波器包围的图像区域中所包 含的像素的排序(排队)为基础，然后使用统计排序结果决定的值代替中心像素的值。这一类中最知名 的滤波器是中值滤波器，正如其名暗示的那样，它是将像素邻域内灰度的中值(在中值计算中包括原像素值)代替该像素的值。
中值滤波器的使用非常普遍，这是因为对于一定类型的随机噪声，它提供 了一种优秀的去噪能力，而且比相同尺寸的线性平滑滤波器的模糊程度明显要低。中值滤波器对处理 脉冲噪声非常有效，该种噪声也称为椒盐噪声，因 这种噪声是以黑白点的形式叠加在图像上的


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241223200215.png)


## 锐化空间滤波器

锐化处理的主要目的是突出灰度的过渡部分
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241223200512.png)
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241223200529.png)


 

### 拉普拉斯算子

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241223200554.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241223200615.png)


### 非锐化掩蔽与高提升滤波

从原图像中减去一幅非锐化（平滑过的）版本
1. 模糊原图像
2. 从原图像中减去模糊图像（产生的差值称为模版）
3. 将模版加到原图像上


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241223201701.png)


### sobel 算子

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241223202743.png)


# 视频转码

## IBP帧

显示顺序与编码顺序
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20241227101634.png)
