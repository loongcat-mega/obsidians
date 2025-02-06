

## 常量的不确定性

在c++11标准以前，我们没有一种方法能够有效地要求一个变量或者函数在编译阶段就计算出结果
const定义的常量和宏都能在要求编译阶段确定 值的语句中使用
预 处理器对于宏只是简单的字符替换，完全没有类型检查，而且宏使用不 当出现的错误难以排查

无论是宏定义的函数调用，还是通过函数返回值初始化const变量，都是在运行时确定的

## constexpr值


constexpr值即常量表达式值，是一个用constexpr说明符声明的变量或数据成员，要求该值必须在编译器计算

在使用常量表达式初始化的情况 下constexpr和const拥有相同的作用。但是**const并没有确保编译期 常量的特性**

**constexpr 说明符（c++11起）声明可以在编译时对函数或变量求值**。这些变量和函数（给定了合适的函数实参的情况下）即可用于需要编译期常量表达式的地方。 

（c++11之前无法保证常量像字面量一样作为数组长度或case语句的标签）

对象或非静态成员函数 (C++14 前)声明中的 constexpr 说明符蕴含 const。函数或静态数据成员 (C++17 起)声明中的 constexpr 说明符蕴含 inline。如果函数或函数模板的一个声明拥有 constexpr 说明符，那么它的所有声明都必须含有该说明符


constexpr 变量必须满足下列要求：

它的类型**必须是字面类型 (LiteralType)**
它必须立即被初始化
它的初始化（包括所有隐式转换、构造函数调用等）的全表达式必须是**常量表达式** 

```cpp
int a=10;
constexpr int b=a;//错误，因为无法用运行期间的变量去初始化一个编译期间的变量

/home/insights/insights.cpp:7:15: error: constexpr variable 'b' must be initialized by a constant expression
    7 | constexpr int b=a;
      |               ^ ~
/home/insights/insights.cpp:7:17: note: read of non-const variable 'a' is not allowed in a constant expression
    7 | constexpr int b=a;
```

c++17之后，lambda表达式在条件允许情况下也可以隐式声明为为constexpr，如下:
```cpp
auto len=[](int i){return 2*i;};  
int arr[len(5)];
```
lambda表达式的值竟然可以作为数组长度

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240429100347.png)




## constexpr函数

非虚
不能是try块

不能是协程(c++20起)

c++11起：
函数体必须被弃置或预置，或只含有下列内容： 
空语句（仅分号）
static_assert 声明
不定义类或枚举的 typedef 声明及别名声明
using 声明
using 指令
当函数不是构造函数时，有恰好一条 return 语句 

c++14起：
函数体必须不含： 
goto 语句
带有除 case 和 default 之外的标号的语句 
try 块
asm 声明
不进行初始化的变量定义 
非字面类型的变量定义
静态或线程存储期变量的定义 


函数返回值是一个编译器就能确定的常量


具有退化机制，当传入的参数不是编译器能确定的常量时，退化为普通非constexpr函数


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240502231731.png)


这段代码虽然能正常运行，但是数组大小并不是在编译期确定，而是运行时确定


## constexpr构造函数

每个被选用于初始化非静态成员和基类的构造函数必须都是 constexpr 构造函数。
构造函数初始化列表必须为常量表达式，且函数体必须为空 --> 字面量类型(c++11标准太过严格了)


```cpp
class A
{
public:
    A():x1(5){}
    int get(){return x1;}
private:
    int x1;
};

constexpr A a;//报错

/home/insights/insights.cpp:18:17: error: constexpr variable cannot have non-literal type 'const A'
   18 |     constexpr A a;
      |                 ^
/home/insights/insights.cpp:8:7: note: 'A' is not literal because it is not an aggregate and has no constexpr constructors other than copy or move constructors
    8 | class A
      |       ^
1 error generated.
Error while processing /home/insights/insights.cpp.

```
解决方案：
```cpp
class A
{
public:
    constexpr A():x1(5){}
    constexpr int get(){return x1;}
private:
    int x1;
};
```


类展开后为：
```cpp
class A
{
  
  public: 
  inline constexpr A()
  : x1{5}
  {
  }
  
  inline constexpr int get()
  {
    return this->x1;
  }
  
  
  private: 
  int x1;
  public: 
};
```


在C++11中，constexpr会自动给函数带上const属性


## constexpr lambda表达式

从C++17开始，lambda表达式在条件允许的情况下都会隐式声明 为constexpr。

```cpp
auto len=[](int i){return 2*i;};
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240502232058.png)

值得注意的是，我们也可以强制要求lambda表达式是一个常量表 达式，用constexpr去声明它即可。这样做的好处是可以检查lambda 表达式是否有可能是一个常量表达式，如果不能则会编译报错

## constexpr的内联属性

c++17中，constexpr声明静态成员变量时，也被赋予了该变量的内联属性

c++17之前，inline static int a=5;只不过是把用到a的地方替换为5，是个纯右值，不能取地址
而c++17之后，这就是一个左值了

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240502232650.png)


## if constexpr
if constexpr是C++17标准提出的一个非常有用的特性，可以用 于编写紧凑的模板代码，让代码能够根据编译时的条件进行实例化

- if constexpr的条件必须是编译期能确定结果的常量表达式
- 条件结果一旦确定，编译器将只编译符合条件的代码块


适用于模版的编写

## 允许constexpr虚函数

减少调用次数？？

## 使用consteval 声明立即函数


c++20引入

我们希望确保constexpr函数在编译期就执行计算，对于无法在编译期执 行计算的情况则让编译器直接报错，因此 使用consteval说明符来声明

```cpp
consteval int  f(int a)
{
    return a*a;
}
int main()
{
    constexpr int b=f(5);
    constexpr int bb=f(6);
    
    int n=20;
    //cin>>n;
    //&n;
    cout<<&A::a<<endl;
    int a[f(5)];
    cout<< sizeof(a)/sizeof(a[0]);
    return 0;
}
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240502233140.png)
下面编译成功的是修改后的

## 总结

我们可以通过constexpr说明符声明 常量表达式函数以及常量表达式值，它们让程序在编译期做了更多的事 情，从而提高程序的运行效率
