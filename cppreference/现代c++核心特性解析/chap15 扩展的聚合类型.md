
## 聚合类型的新定义

c++17：从基类公开且非虚继承的类也可能是一个聚合，同时 聚合类型还需要满足常规条件：
- 没有用户提供的构造函数
- 没有私有和受保护的非静态数据成员
- 没有虚函数

在新的扩展中，如果存在继承关系，则额外满足以下条件：
- 必须是公开的基类，不能有私有或者保护的基类
- 必须是非虚继承

可以使用is_aggregate_v\<T\> 来判断 一个类是不是聚合类

## 聚合类初始化

**参数列表初始化**，即大括号，初始化顺序按继承基类顺序+派生类成员数据顺序初始化






```cpp
#include<iostream>
using namespace std;
class A
{
public:
    int a=0;
    A(int a_):a(a_){};
};
class B:public A
{
public:
    //using A::A;
    int b=0;
};
int main()
{
    cout<<is_aggregate_v<A> <<endl;
    cout<<is_aggregate_v<B> <<endl;
    B b{10,20};
    cout<<b.a<<endl;
    cout<<b.b<<endl;
    return 0;
}
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240501215750.png)

对一个聚合类使用小括号初始化，会调用类的构造函数，但是聚合类没有自定义构造函数，会编译错误

