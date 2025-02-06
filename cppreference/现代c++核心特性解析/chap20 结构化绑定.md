
c++17引入

结构化绑定声明将标识符列表 ﻿中的所有标识符引入作为其外围作用域中的名字，并将它们绑定到表达式 ﻿所指代的对象的各个子对象或元素。以此方式引入的绑定被称作**结构化绑定**。

在结构化绑定中 编译器会根据限定符生成一个等号右边对象的匿名副本，而绑定的对象正是这个副本而非原对象本身



## 绑定到多个返回值

```cpp
#include<iostream>
#include<tuple>
using namespace std;
auto mv(int a,int b)
{
    return make_tuple(a*b,a+b);
}
int main()
{
   auto [ra,rb]=mv(10,20);
   cout<<ra<<","<<rb<<endl;
}
```

将列表中的ra rb绑定到mv的返回值上

## 绑定到结构体和类对象

非静态成员和列表参数个数一致
必须是public
不能绑定方法
非静态数据成 员要么全部在派生类中定义，要么全部在基类中定义

```cpp
#include<iostream>
#include<tuple>
using namespace std;
auto mv(int a,int b)
{
    return make_tuple(a*b,a+b);
}
class A
{
public:
    int a=10;
    int b=20;
    A(int aa=10,int bb=20)
    {
        a=aa;
        b=bb;
    }
};
int main()
{
   auto [ra,rb]=mv(10,20);
   cout<<ra<<","<<rb<<endl;
   auto [a,b]=A(5);
    cout<<a<<","<<b<<endl;
}
```

这里的结构体绑定其实是生成了一个结构体副本
```cpp
A AA(5);  
auto [a,b]=AA;  
cout<<a<<","<<b<<endl;  
a=50;  
cout<<AA.a<<","<<AA.b<<endl;
```
修改绑定的值并不会影响原来结构体的值

## 绑定到数组

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240428165420.png)
