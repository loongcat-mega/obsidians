## 左值和右值


左值右值针对表达式说的


能够取地址的，有名字的就是左值；反之，不能取地址，没有名字的就是右值。`
在C++中所谓的左值一般是指一个指向特定内存的具有名称的值 （具名对象），它有一个相对稳定的内存地址，并且有一段较长的生命 周期。而右值则是不指向稳定内存地址的匿名值（不具名对象），它的 生命周期很短，通常是暂时性的

左值引用无法引用右值
右值引用也无法引用左值



```c++
i++; 右值 不能取地址
++i; 左值 能取地址
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240428203036.png)


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240429185427.png)


通常字面量都是一个右值，除字符串字面量以外

编译器会将字符串字面量存储到程序的数据段中，程序加载的时候也会 为其开辟内存空间，所以我们可以使用取地址符&来获取字符串字面量 的内存地址。

 以下代码基于c++14
```cpp
//
// Created by dlut2102 on 2024/4/28.
//
#include<iostream>
using namespace std;
class A
{
public:
    A(){cout<<"默认构造"<<endl;}
    A(const A&a){cout<<"拷贝构造"<<endl;}
    ~A(){cout<<"析构"<<endl;}
};
A get()
{
    A a;
    cout<<"get:"<<&a<<endl;
    return a;
}
int main()
{
    A a=get();
    cout<<"main:"<<&a<<endl;
    
    return 0;
}
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240428205728.png)

 
## 左值引用

当我们需要将一个对象作为参数传递给子函数的时候，往往会使用左值引用，**因为这样可以免去创建临时对象的操作**
```c++
int &a=b;
//b修改，a同步修改
```


常量左值引用可以引用右值

```cpp
const int &a=10;
```


## 右值引用

右值引用是一种引用右值且只能引用右值的方法

可以延长变量的生命周期


```c++
pair<int,int>hashmap[10];
auto&&[fst,snd]=hashmap[1];
//修改fst 与 snd的值，hashamap[1]的值同步修改
```


常量左值引用不止能引用左值，还能引用右值
```c++
const int&a=10;
const int&b=a;

```
延长临时对象生命周期并不是 这里右值引用的最终目标，其真实目标应该是减少对象复制，提升程序 性能。

## 右值的性能优化空间




很多情况下右值都存储在临时对象 中，当右值被使用之后程序会马上销毁对象并释放内存

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240429232608.png)

会产生三次拷贝构造调用，但是有两次都是拷贝的临时对象，浪费时间和空间
在这里每发生一次复制构造都会复制整整4KB的数 据，如果数据量更大一些，比如4MB或者400MB，那么将对程序性能造 成很大影响


## 移动语义

如果有办法能将临时对 象的内存直接转移到my_pool对象中，不就能消除内存复制对性能的消 耗吗

复制构造器(&)：左值引用
移动构造器(&&)：右值引用，比如要拷贝一个临时对象的内容，拷贝不如移动来的快

```cpp
movector(A &&a)
{
		this->ptr=a.ptr;
		a.ptr=nullptr;
}
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240429232741.png)

上述调用会产生一次拷贝调用，两次移动调用，移动只是对指针的操作，并不会消耗太多时间

对 于复制构造函数而言形参是一个左值引用，也就是说函数的实参必须是 一个具名的左值，在复制构造函数中往往进行的是深复制，即在不能破 坏实参对象的前提下复制目标对象。而移动构造函数恰恰相反，它接受 的是一个右值，其核心思想是通过转移实参对象的数据以达成构造目标 对象的目的，也就是说实参对象是会被修改的

但是，使用移动构造会有一些风险，试想一下，在一个移动构造函数中，如果当一 个对象的资源移动到另一个对象时发生了异常，也就是说对象的一部分 发生了转移而另一部分没有，这就会造成源对象和目标对象都不完整的 情况发生，这种情况的后果是无法预测的。


## 将左值转换为右值

**让左值 使用移动语义**

```cpp
BigMemoryPool my_pool1;
BigMemoryPool my_pool2 = my_pool1; 
BigMemoryPool my_pool3 = static_cast<BigMemoryPool &&>(my_pool1);
```

这里使用了 static_cast<BigMemoryPool &&>(my_ pool1)将my_pool1强制转 换为右值（也是将亡值，为了叙述思路的连贯性后面不再强调）。由于 调用了移动构造函数，my_pool1失去了自己的内存数据，后面的代码 也不能对my_pool1进行操作了。

无论一个函数的实参是左值 还是右值，其形参都是一个左值，即使这个形参看上去是一个右值引 用
`std::move` 函数来显式地将左值转换为右值




## 完美转发

函数传参的时候可以传引用，但是如果形参是个右值引用，就不能使用了，->常量左值引用

将参数变成右值，然后用模版万能引用接受

...

等遇到了再继续补充

## 针对局部变量和右值引用的隐式移动操作
```cpp
#include<bits/stdc++.h>
using namespace std;
class A
{
public:
    int *p;
    A()
    {
        p=new int(10);
        cout<<"default ctor\n";
    }
    ~A()
    {
        cout<<"delete ctor\n";
        
        if(p==nullptr)
            delete p;
    }
    A ( const A& a)
    {
        cout<<"copy ctor\n";

        *p=*(a.p);
        //return *this;
    }
    A operator= (const A &&a)
    {
        cout<<"equal ctor\n";
        *p=*(a.p);
        return *this;
    }
    A( A&& a)
    {
        cout<<"move ctor\n";
        
        p=a.p;
        a.p=nullptr;
    }
};
A getA()
{
    A a;
    cout<<"getA() :"<<a.p <<endl;  
    return a;
}
int main()
{
    A a=getA();
    cout<<"main() :"<<a.p <<endl;

    return 0;
}
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240429233724.png)
从代码上看，赋值应该是一个复制，对于旧 时的标准这是没错的。但是对于支持移动语义的新标准，这个地方会隐 式地采用移动构造函数来完成数据的交换。编译运行以上代码最终会显 示move ctor字符串。

右值赋给左值，会调用移动构造


## 总结

右值引用是C++11标准提出的一个非常重要的概念，它的出现不仅 完善了C++的语法，改善了C++在数据转移时的执行效率，同时还增强 了C++模板的能力

常用的容器vector、list和map等均已支持移动构造函数和移动赋值运 算符函数