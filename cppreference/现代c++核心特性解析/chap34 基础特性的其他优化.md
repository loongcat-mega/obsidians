
## 显示自定义类型转换运算符c++11

```cpp
//
// Created by dlut2102 on 2024/5/5.
//
#include<vector>
#include<iostream>
using namespace std;
template<class T>
class Storage
{
public:
    Storage()=default;
    Storage(initializer_list<T> l):date__(l){};
    operator bool() const {return !date__.empty();};
private:
    vector<T> date__;
};
int main()
{
    Storage<float> s1{1.,2.,3.};
    Storage<int> s2{1,2,3};
    cout<<boolalpha;
    cout<<static_cast<bool>(s2)<<endl;
    cout<<(s1==s2)<<endl;
    cout<<(s1+s2)<<endl;
    return 0;
}
```
上述代码输出为true和2
因为由于重载了(),所以在比较的时候发生了隐式类型转换

下面例子更直观
```cpp
class A
{
public:
    A()=default;
    A(int a_):a(a_){};
    int a=10;
    
};
A get_A(A a)
{
    return a;
}
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240505165917.png)

是可以编译通过的，1发生隐式类型转换为A类型

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240505170012.png)

explicit说明符将构造函数声明为显式，这样隐式的构造无法通过 编译,不支持隐式调用，只能显式调用



```cpp
class Storage  
{  
public:  
    Storage()=default;  
    Storage(initializer_list<T> l):date__(l){};  
    explicit operator bool() const {return !date__.empty();};  
private:  
    vector<T> date__;  
};
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240505170148.png)

**'explicit' can only be specified inside the class definition
'explicit' can only be applied to a constructor or conversion function**


C++11标准将explicit引入自定义 类型转换中，称为显式自定义类型转换。语法上和显式构造函数如出一 辙，只需在自定义类型转换运算符的函数前添加explicit说明符

## 返回值优化

编译优化技术，允许编译器将函数返回的对象直接构造到他们本来要存储的变量空间中而不产生临时对象

分为RVO(Return Value Optimization)和NRVO(Named Return Value Optimization)
当返回语句的操作数为临时对象时，称之为RVO
当返回语句的操作对象为具名对象时，称之为NRVO

如果使 用GCC作为编译器，则这项优化技术是默认开启的，取消优化需要额外 的编译参数“-fno-elide- constructors”。

C++14标准对返回值优化做了进一步的规定，规定中明确了对于常 量表达式和常量初始化而言，编译器应该保证RVO，但是禁止NRVO。

## volatile

volatile是一个非常著名的关键字，用于表达易失性。它能够让 编译器不要对代码做过多的优化，保证数据的加载和存储操作被多次执 行，即使编译器知道这种操作是无用的，也无法对其进行优化。

事实 上，在现代的计算机环境中，volatile限定符的意义已经不大了。首 先我们必须知道，该限定符并不能保证数据的同步，无法保证内存操作 不被中断，它的存在不能代替原子操作。其次，虽然volatile操作的 顺序不能相对于其他volatile操作改变，但是可以相对于非volatile 操作改变。更进一步来说，即使从C++编译代码的层面上保证了操作执 行的顺序，但是对于现代CPU而言这种操作执行顺序也是无法保证的。

## 逗号运算符

它可以让多个表达式按照从左 往右的顺序进行计算，整体的结果为系列中最后一个表达式的值

不推荐在下标 表达式中使用逗号运算符
```cpp
int a[]{ 1,2,3 };
int x = 1, y = 2;
std::cout << a[x, y];
```

