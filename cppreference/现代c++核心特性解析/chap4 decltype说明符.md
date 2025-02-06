
[decltype 说明符](https://zh.cppreference.com/w/cpp/language/decltype#Popover19-toggle:~:text=decltype%20%E8%AF%B4%E6%98%8E%E7%AC%A6)
推导实体和表达式类型

## typeof和typeid

在C++11标准发布以前，GCC的扩展提供了一个名为typeof的运算 符。通过该运算符可以获取操作数的具体类型
```cpp
int a=10;
typeof(a) b=10;
```
typeid 可以返回一个type_info类型的对象，记录了类型的标识符和类型名
只能在运行时做类型甄别，不能在编译器识别

```cpp
cout<<typeid(typeid(int)).name()<<endl;//N10__cxxabiv123__fundamental_type_infoE
cout<<typeid(int).name()<<endl;//i
cout<<typeid(double).name()<<endl;//d
cout<<typeid(float).name()<<endl;//f
cout<<typeid(bool).name()<<endl;//b
cout<<typeid(char).name()<<endl;//c
cout<<typeid(string).name()<<endl;//NSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEE
int arr[5]={1,2,3};  
double arr1[5]={1,2,3};  
cout<<typeid(arr).name()<<endl;//A5_i  
cout<<typeid(arr1).name()<<endl;//A5_d
```

- typeid的返回值是一个左值
- typeid返回的std::type_info删除了复制构造函数，如果保存该变量，则只能获取其引用或指针
- typeid的返回值总是忽略类型的cv限定符

typeid不能像typeof那样在编译器就确定对象类型

## 使用decltype说明符


decltype用法与typeof类似，能够鉴别对象或表示式的类别

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240428201152.png)


在C++中，“&&”符号表示右值引用类型。它通常与移动语义相关联，允许在不必要复制的情况下有效地从一个对象转移资源（如内存）到另一个对象。

```cpp
const int && foo(){return 40;}
struct A
{
    double a;
};
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240428202441.png)


const右值引用无法绑定到左值
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240428203509.png)

但是可以这么修改
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240428203645.png)



为了更好地讨论decltype的优势，需要用到函数返回类型后置

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240429231024.png)

利用decltype说明符在编译器完成类型的推导，auto做返回值占位符，实际返回类型是decltype(a1+a2)

## 推导规则

decltype(e)，e的类型为T:

- 如果e是一个未加括号的标识符表达式，或者未加括号的类成员访问，则decltype(e)推导出的类型是e的类型T
- 如果e是一个函数调用或者仿函数调用，那么decltype(e)是其返回值的类型
- 如果e是一个类型为T的左值，则decltype(e)是T&
- 如果e是一个类型为T的将亡值，那么decltype(e)是T&&
- 除去以上情况，decltype(e)是T

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240429231448.png)


## cv限定符的推导

通常情况下，decltype(e)推导的类型会同步e的cv限定符
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240429231545.png)

但是，当e是未加括号的成员变量时，父对象表达式的cv限定符会被忽略
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240429231617.png)

当e是加括号的数据成员时，父对象表达式的cv限定符会同步到推断结果

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240429231706.png)

## 总结

decltype和auto的使用方式有一些相似之处，但是推导规则却有 所不同，理解起来有一定难度。不过幸运的是，大部分情况下推导结果 能够符合我们的预期。另外从上面的示例代码来看，在通常的编程过程 中并不会存在太多使用decltype的情况。实际上，decltype说明符对 于库作者更加实用。因为它很大程度上加强了C++的泛型能力