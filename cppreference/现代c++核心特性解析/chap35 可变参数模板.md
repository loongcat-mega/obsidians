
 ## 可变参数模板的概念和语法

可变参数模板是C++11标准引入的一种新特性，顾名思义就是**类模 板或者函数模板的形参个数是可变的**


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240505184601.png)

`class ...Args`是**类型模板形参包**，可以接受零个或者多个类型模板实参

`Args ...args`	叫做**函数形参包**，出现在函数列表中，接受零个或者多个函数实参

`args ...` 是**形参包展开**，通常简称包展开。它将形参包展开为零个或者多个模式的列表，这个过程称为解包

所谓的**模式就是实参展开的方法**，形参包的名称必须出现在这个方法中作为实参展开的依据，最简单的情况为解包后就是实参本身

**在类模板中，模板形参包必须是模版形参列表的最后一个形参**

**但是对于函数模板而言，模版形参包不必出现在最后，只要保证后续的形参类型能够通过实参推导或者具有默认形参即可**


## 形参包展开

包展开并不是在所有情况下都能够 进行的，允许包展开的场景包括以下几种
- 表达式列表
- 初始化列表
- 基类描述
- 成员初始化列表
- 函数参数列表
- 模板参数列表
- 动态异常列表
- lambda表达式捕获列表
- sizeof... 运算符
- 对齐运算符
- 属性列表

```cpp
template<class T, class U> 
T baz(T t, U u) 
{ 
 std::cout << t << ":" << u << std::endl;
 return t; 
} 

template<class ...Args> 
void foo(Args ...args){} 

template<class ...Args> 
class bar
{ 
public: 
    bar(Args ...args)
    {  
        foo(baz(&args, args) ...);
    } 
}; 
void main() 
{
 bar<int, double, unsigned int> b(1, 5.0, 8);
}
```

baz 普通函数模板
foo 可变参数函数模板

可变参数类bar中，构造函数`bar(Args ...args)` 接受一个可变参数列表，`foo(baz(&args, args) ...);`对形参包进行了展开：`baz(&args,args)...`是包展开，`baz(&args,args)`就是模式，可以理解为包展开的方法


## sizeof... 运算符

sizeof运算符能获取某个对象类型的字节大小
而sizeof... 是专门针对形参包引入的新运算符，目的是获取形参包中形参的个数，返回的类型是std::size_t


## 可变参数模板的递归计算

```cpp
template<class T> 
T sum(T arg) 
{ 
		return arg; 
}

template<class T1, class... Args> 
auto sum(T1 arg1, Args ...args)
{ 
		return arg1 + sum(args...); 
}

```
当传入模板参数的实参量为1时，编译器会选择调用`T sum(T args)` 
当传入模板实参的参数量大于1时，编译器会选择调用`auto sum(T1 arg1,Args ...args)` 这里使用c++14的特性，将auto作为占位符，返回类型推导交给编译器。

## 折叠表达式

递归计算的方式过于烦琐，数组和 括号表达式的方法技巧性太强也不是很容易想到。为了用更加正规的方 法完成包展开，C++委员会在C++17标准中引入了折叠表达式的新特 性。让我们使用折叠表达式的特性改写递归的例子

```cpp
template <class ...T>
auto get_sum(int Case,T... t)
{
    switch(Case)
    {
        case 1:
            //二元向左折叠
            return (1 + ... + t );
            break;
        case 2:
            
            //二元向右折叠
            return ( t + ... + 1 );
            break;
        case 3:
            //一元向右折叠
            return (t + ...);
            break;
        default:
            //一元向左折叠
            return (... + t );
    }
   
}

```

一元向左折叠：
`( ... op args )折叠为((((arg0 op arg1) op arg2) op ...) op argN)`

一元向右折叠
`( args op ... )折叠为(arg0 op (arg1 op ... (argN-1 op argN)))`


二元折叠总体上和一元相同，唯一的区别是多了一个初始值，比如 二元向右折叠：

`( args op ... op init )折叠为(arg0 op (arg1 op ...(argN-1 op (argN op init)))`

二元向左折叠
`( init op ... op args )折叠为(((((init op arg0) op arg1) op arg2) op ...) op argN)`

**op则代表任意一个二元运算符。值得注意的是，在二元折 叠中，两个运算符必须相同**

```cpp
std::cout << gte_sum(3,std::string("hello "), "c++ ", "world") << std::endl;
```

采用一元向左折叠方式运行上述代码，会报错，因为原生字面量字符串不允许+操作


## using声明中的包展开

用于可变参数类模板派生于形参包



![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240505191911.png)


## lambda表达式初始化捕获的包展开

## 总结

可变参数模板特性，该特性可以说是新标准中最重 要的模板相关的特性
