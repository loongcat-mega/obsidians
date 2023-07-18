包含头文件与命名空间
```c++
#include"Eigen/Dense"
using namespace Eigen
```
## 矩阵类Matrix
```c++
Matrix<typename Scalat,int RowAtComlileTime,int ColsAtComlileTime>
// 类型 行 列

//参数编译时已知
typedef Matrix<float,4,4> Matrix4f;
typedef Matrix<float,3,1> Vector3f;
typedef Matrix<int,1,2> RowVector2i;


//参数编译时未知
typedef Matrix<double,Dynamic,Dynamic> MatrixXd;
typedef Matrix<int,Dynamic,1> VectorXi;


Matrix3f a;
//3*3单精度浮点型矩阵，内部元素没有被初始化
MatrixXf d;
//动态矩阵，还没为该矩阵分配内存


MatrixXf a(10,15);
//10*15的动态矩阵，内存进行了分配，但还没进行初始化
VectorXf b(30);
//大小为30的动态数组，但没有初始化


//通过重载（）访问矩阵或者向量的元素，序号从0开始，除向量外，不支持通过[]访问矩阵元素
MatrixXd m(2,2);
m(0,0)=3;
VectorXd v(2);
v(0)=4;

Matrix3f m;
m<<1,2,3,
	4,5,6,
	7,8,9;
cout<<m;


//可以使用rows(),cols(),size()访问矩阵当前大小，使用resize()重置矩阵大小，

```
#### 矩阵和向量代数
```c++
//转置 transpose()
//转置自己 transposeInPlace();只能转置方阵
//共轭 conjuagte()
//共轭转置 adjoint() 伴随矩阵

//点乘 dot();
//叉乘 cross();只适合三维向量

sum();//计算所有矩阵元素的和
prod();//计算所有元素连乘积
mean();//计算矩阵元素的平均值
minCoeff();//计算矩阵元素最小值
maxCoeff();//计算矩阵元素最大值
trace();//计算矩阵的迹 所有对角元之和
determinant() ;// 求行列式

norm();//向量求模，矩阵范数

```

#### 随机向量或矩阵生成
```c++
MatrixXd m=MatrixXd::Random(2,3);
VectorXi v=VectorXi::Random(1);

//零矩阵 Zero()函数
MatrixXd m=MatrixXd::Zero(2,3);
Vectori v=VectorXi::Zero(1);

//单位矩阵 等价标准型 Identity()函数
MatrixXd m=MatrixXd::Identity(5,5);

```
#### 块操作

```c++
block();
//动态大小块
block(i,j,p,q);
//固定大小块
block<p,q>(i,j);
//i,j表示块的起始位置，p，q表示块的大小

//可作左值，也可作右值
```