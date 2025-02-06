
c++11引入


既是说明符，又是运算符
只关心是否抛出异常

作为说明符，在函数末尾添加noexpect即可，表示该函数声明为不会抛出异常的函数，如果函数抛出异常，则会终止程序

由于noexcept对表达式的评估是在编译阶段执 行的，因此表达式必须是一个常量表达式
noexpect可以接受一个bool类型的常量表达式，决定该函数是否可以抛出异常，默认情况是true

作为运算符，noexpect(f)表示该函数是否可以抛出异常

```cpp
#include<iostream>
using namespace std;
void f() noexcept{}
void ff(){}
int main()
{
    cout<<boolalpha;
    cout<<noexcept(f())<<endl;//true
    cout<<noexcept(ff())<<endl;//false
    return 0;
}
```

为了解决移动构造问题？(c++11引入)

默认构造、默认拷贝构造、默认赋值函数、默认移动构造函数、默认移动赋值函数、默认析构函数等都默认使用noexpect
使用noexpect的对应的函数在类型的基类和成员中也具有noexpect声明




```cpp
template <class T>  
T copy(const T &o) noexcept(noexcept(T(o))){}
```
第二个noexpect是一个运算符，判断是否T(o)可能抛出异常，如果抛出异常，noexpect(T(o))值为false
第一个noexpect是一个说明符，表示该函数是不是会抛出异常，如果参数为true，说明不会抛出异常

## 使用noexpect来解决移动构造问题

异常的存在对容器数据的移动构成了威胁，因为我 们无法保证在移动构造的时候不抛出异常。现在noexcept运算符可以 判断目标类型的移动构造函数是否有可能抛出异常。如果没有抛出异常 的可能，那么函数可以选择进行移动操作；否则将使用传统的复制操 作。

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240501224430.png)


检查T类型的移动构造函数(noexpect(move(a)))和移动赋值函数(noexpect(a= move(b)))是否都不会抛出异常



但是这个函数并没有解决上面容器移动的问题，继续改进swap函数：

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240501224747.png)

使用静态断言，断定移动不会抛出异常


但是这种实现方式过于强势，我们希望在不满足 移动要求的时候，有选择地使用复制方法完成移动操作。最终版swap函数：

```cpp
template<typename T> 
void swap(T& a, T& b)
noexcept(noexcept(swap_impl(a, b, std::integral_constant<bool, noexcept(T(std::move(a))) 
        && noexcept(a.operator=(std::move(b)))>())))
{ 
    swap_impl(a, b, std::integral_constant<bool, noexcept(T(std::move(a))) 
            &&noexcept(a.operator=(std::move(b)))>()); 
}
```
如果移动赋值不会抛出异常，则noexcept(T(std::move(a))返回值为true，如果移动赋值和移动构造都没有异常，则参数为true，调用移动版，此时函数是不会抛出异常的，即noexpect(true)


swap_impl为重载的移动赋值和赋值赋值函数

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240501225254.png)



## 默认使用noexpect的函数

- 默认构造
- 默认复制构造
- 默认赋值
- 默认移动构造
- 默认移动赋值

对应的函数在类型的基类和成员中也具有noexpect声明，否则其对应函数将不再带有noexpect声明
**自定义实现的函数默认也不会带有noexpect声明**

类型的析构函数以及delete运算符默认带有noexcept声明，请 注意**即使自定义实现的析构函数也会默认带有noexcept声明**，除非类 型本身或者其基类和成员明确使用noexcept(false)声明析构函数， 以上也同样适用于delete运算符
## 使用noexpect时机

**使用时要谨慎，确保函数一定不会抛出异常才使用noexpect声明**

或者

**抛出异常就直接终止**

## 总结

noexcept不仅是 说明符同时也是运算符，它既能规定函数是否抛出异常也能获取到函数 是否抛出异常，这一点让程序员有办法更为灵活地控制异常

