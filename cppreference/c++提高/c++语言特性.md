
c++不仅具备面向对象的常规语言要素，如类 继承 多态 流 异常等机制，还包括诸多c++特有的语言要素，如多继承、复制构造、运算符重载、指针、引用、模板等

将数据和对数据的操作视为一个相互依赖 不可分割的对象，采用数据抽象和数据隐藏的技术


# c++ 对 c 的扩展


| 扩展       | 描述                                                                                                                   | 注意点                                     |
| ---------- | ---------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| 函数重载   |                                                                                                                        |                                            |
| 操作符重载 |                                                                                                                        |                                            |
| 默认参数   |                                                                                                                        | 一个函数，不能既作重载，又作默认参数的函数 |
| 引用       |                                                                                                                        |                                            |
| new/delete | new/delete 是关键字，效率高于 malloc 和 free.申请的时候会调用构造器完成初始化，释 放的时候，会调用析构器完成内存的清理 |                                            |
| inline     | 避免调用时的额外开销,适用于函数“小”，且被频繁调用场景                                                                  |                                            |
| 类型转换   |                                                                                                                        |                                            |
| 命名空间   | 命名空间为了大型项目开发，而引入的一种避免命名冲突的一种机制                                                           |                                            |
| string类   |                                                                                                                        |                                            |
| 内存管理：智能指针           |                                                                                                                        |                                            |


## const

修饰符，告诉编译器被修饰的东西只读，const修饰的对象**必须在定义时初始化，只能在初始化列表中赋值**

const类成员函数不能调用非const类成员函数

指向const变量的指针`const int *p*=&a`,可以改变指针指向，但不能改变指针指向的值

const指针` int* const p=&a` 不可以改变指针指向

const与非const能够重载

1，如果 const 构成函数重载，const 对象只能调用 const 函数，非 const 对象优先调 用非 const 函数。
2，const 函数只能调用 const 函数。非 const 函数可以调用 const 函数。 
3，类体外定义的 const 成员函数，在定义和声明处都需要 const 修饰符。



![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240503100836.png)



## static


1，static 成员变量实现了**同簇类对象间信息共享**。
2，static 成员类外存储，求类大小，并不包含在内。
3，static 成员是命名空间属于类的全局变量，存储在 data 区 rw 段。
4，static 成员使用时必须实始化，且只能类外初始化(c++14引入inline)。
5，可以通过类名访问（无对象生成时亦可），也可以通过对象访问。


- 局部静态变量

生命周期是整个程序执行期间
一次定义，多次使用，且只能定义一次

- 全局静态变量

在文件作用域内定义的静态变量，它只对该文件可见，其他文件无法直接访问它，有助于实现信息封装

-  静态类成员

属于类本身，所有类的实例共享一个静态成员变量

- 静态函数

类内定义的静态函数，只能访问类的静态成员和其他静态成员函数，不能访问非静态成员。静态成员函数也不依赖于类的特定实例


**如果一个类的成员，既要实现共享，又要实现不可改变，那就用 static const 修饰。 修饰成员函数，格式并无二异，修饰数据成员。必须要类内部实始化**




## 引用
```cpp
int a=10;  
const int &b=20;  
cout<<a<<" "<<b<<endl;  
a=30;  
cout<<a<<" "<<b<<endl;
```

```txt
10 20
30 20
```
实际上，const 引用使用相 关类型对象初始化时发生了如下过程：
```cpp
int temp = val; 
const int &ref = temp;
```

```cpp
int a=10;  
const int &b=20;  
int &c=a;  
cout<<a<<" "<<b<<endl;  
cout<<&a<<" "<<&b<<" "<<&c<<endl;  
a=30;  
cout<<a<<" "<<b<<endl;  
cout<<&a<<" "<<&b<<" "<<&c<<endl;
```
```txt
10 20
0x482a3ff618 0x482a3ff61c 0x482a3ff618
30 20
0x482a3ff618 0x482a3ff61c 0x482a3ff618
```

## 类型转换
### 静态类型转换

在一个方向上可以作隐式转换，在另外一个方向上就可以作静态转换
`static_cast<目标类型> (标识符)`

用于类层次结构中基类和派生类之间引用或指针的转换。
进行**上行转换（把派生类的指针或引用转换成基类表示）是安全**的。
进行下行转换（把基类的指针或引用转换成派生类表示），由于没有动态类型检查，不安全。
用于基本数据类型之间的转换
把空指针转换成目标类型的空指针
把任何类型的表达式转换成void类型

## 动态类型转换

用于多态中的父子类之间的强制转化
`dynamic_cast<目标类型> (标识符)`


## 命名空间

Global scope 是无名的命名空间
如何访问被局部变量覆盖的全局变量？
```cpp
int g_val=10;  
int main()  
{  
    int g_val=20;  
    cout<<"local scope "<<g_val<<endl;  
    cout<<"global scope "<<::g_val<<endl;  
    return 0;  
}
```
```txt
local scope 20
global scope 10
```

不可以用局部变量去接受全局变量
```cpp
int g_val=10;
int main()
{
    int rg_val=g_val;
    int g_val=20;
    cout<<"local scope "<<g_val<<endl;
    cout<<"global scope "<<::g_val<<endl;
    cout<<"rglobal scope "<<rg_val<<endl;
    rg_val=50;
    cout<<"global scope "<<::g_val<<endl;
    cout<<"rglobal scope "<<rg_val<<endl;
    
    return 0;
}
```
```txt
local scope 20
global scope 10
rglobal scope 10
global scope 10
rglobal scope 50
```

但是可以用引用

## new/malloc delete/free

都是在堆上管理内存的方式

new更符合面向对象的思想，提供了更方便、安全的内存分配方式，并且支持构造函数的自动调用



### 返回对象
new 返回的是分配对象类型的指针
malloc 返回的是void* 类型指针，需要进行显式类型转换

### 构造函数
new会调用调用对象的构造函数，会自动初始化
malloc则不会，需手动调用构造函数


### 内存分配
new只需传入对象，根据对象自动计算大小
malloc需要显式指定所需内存大小

### 异常处理

new会抛出bad_alloc异常
malloc只返回NULL

## 指针与引用

### 初始化

指针可以不用初始化
引用必须初始化

### 可变性

指针的值可以改变
而引用的值不能改变

### 内存分配

指针本身需要分配内存
而引用不需要额外的内存分配，他只是变量的别名

### 用途

引用传参可以减少拷贝


## 圣经



1、在 C++中**几乎不需要用宏**，用 **const 或 enum 定义显式的常量**，用 **inline 避免函数 调 用的额外开销**，用**模板去刻画一族函数或类型**，用 **namespace 去避免命名冲突**。 
2、不要在你需要变量之前去声明，以保证你能立即对它进行初始化。
3、不要用 malloc,new 运算会做的更好。
4、避免使用 void*、指针算术、联合和强制，大多数情况下，强制都是设计错误的指示器。 
5、**尽量少用数组和 C 风格的字符串，标准库中的 string 和 vector 可以简化程序**。 
6、更加重要的是，试着将程序考虑为一组由类和对象表示的相互作用的概念，而不是一 堆数据结构和一些可以拨弄的二进制。



# 封装 Encapsulation


class 封装的本质，在于**将数据和行为，绑定在一起然后通过对象来完成操作**，目的是把类的设计和使用分开来

类属性和类函数是分开存放的，类属性是用struct结构体来存放的，类函数是全局存放的

## 构造器

在对象创建时自动调用,完成初始化相关工作。
无返回值，与类名同
可以重载，可默认参数
默认无参空体，一经实现，默认不复存在


### 初始化列表

初始化列表中的初始化顺序，与声明顺序有关，与前后赋值顺序无关
必须用此格式来实始化引用数据
必须用此格式来初始化非静态 const 数据成员

下面这段代码有错误：
```cpp
#include <iostream>
#include <string.h> 
using namespace std;
class A
{
public: 
    A(string&& ps) :name(ps),len(name.size()){}
    void dis() { cout<<len<<endl; } 
private:
    int len;
    string name;
};

int main() 
{ 
    A a("china"); 
    a.dis(); 
    return 0; 
}
```

```txt
Field 'name' is uninitialized when used here
```

将len与name声明顺序反一下就可以
```cpp
string name;  
int len;
```
## 析构器

析构函数的作用，并不是删除对象，而在对象销毁前完成的一些清理工作


## 拷贝构造


```txt
类名(const 类名 & another)
```

系统提供默认的拷贝构造器，一经定义不再提供。但系统提供的默认拷贝构造器是等位 拷贝，也就是通常意义上的浅拷贝。如果类中包含的数据元素全部在栈上，浅拷贝也可以满 足需求的。但如果堆上的数据，则会发生多次析构行为

拷贝构造调用时机：
1，制作对象的副本。
2，以对象作为参数和返回值。

## this  指针

系统在创建对象时，默认生成的指向当前对象的指针。这样作的目的，就是为了带来方 便

## 运算符重载 =

```txt
类名& operator=(const 类名& 源对象)
```

返回引用，且不能用 const 修饰


## 栈上返回对象

```cpp
#include <iostream>  
#include <string.h>   
using namespace std;  
int con=0;  
int cpy=0;  
int ope=0;  
int decon=0;  
class A  
{  
public:  
    A(){cout<<"this is construction\n";con++;};  
    A(const A&){cout<<"this is copy-construction\n";cpy++;};  
    A& operator=(const A&){cout<<"this is copy-operator\n";ope++;return *this;};  
    ~A(){cout<<"this is de-construction\n";decon++;};  
};
```

---


```cpp
A get_A(A&a)
{
    //A a;
    cout<<"this is get_A() "<<&a<<endl;
    return a;
}

int main() 
{
    {
        A t;
//        A b = 
        get_A(t);
        cout << "this is main() " << &t << endl;
    }
   
    cout<<"------------------------\n";
    cout<<"construction times:"<<con<<endl;
    cout<<"copy-construction times:"<<cpy<<endl;
    cout<<"copy-operator times:"<<ope<<endl;
    cout<<"de-construction times:"<<decon<<endl;
    return 0; 
}
```

```txt
this is construction
this is get_A() 0xb88d3ffd6e
this is copy-construction
this is de-construction
this is main() 0xb88d3ffd6e
this is de-construction
------------------------
construction times:1
copy-construction times:1
copy-operator times:0
de-construction times:2




```

---


```cpp
A get_A(A&a)
{
    //A a;
    cout<<"this is get_A() "<<&a<<endl;
    return a;
}

int main() 
{
    {
        A t;
        A b = 
        get_A(t);
        cout << "this is main() " << &t << endl;
    }
   
    cout<<"------------------------\n";
    cout<<"construction times:"<<con<<endl;
    cout<<"copy-construction times:"<<cpy<<endl;
    cout<<"copy-operator times:"<<ope<<endl;
    cout<<"de-construction times:"<<decon<<endl;
    return 0; 
}
```

```txt
this is construction
this is get_A() 0x86c75ff75f
this is copy-construction
this is main() 0x86c75ff75f
this is de-construction
this is de-construction
------------------------
construction times:1
copy-construction times:1
copy-operator times:0
de-construction times:2

```

A a = get_A(t); 和 A a(get_A(t)); 并不完全等价。

A a = get_A(t); 使用的是复制初始化（copy initialization），这会调用拷贝构造函数来初始化 a。首先，get_A(t) 返回一个临时对象，然后通过拷贝构造函数将这个临时对象的值复制给 a。
A a(get_A(t)); 使用的是直接初始化（direct initialization），这不会调用拷贝构造函数。而是直接调用 A 类型的构造函数，将 get_A(t) 返回的对象直接传递给 A 类型的构造函数来创建 a 对象。
虽然这两种方式在某些情况下可能会产生相同的结果，但它们的行为是不同的。一般来说，直接初始化更高效，因为它避免了临时对象的创建和拷贝构造函数的调用。


---

```cpp
{  
    A t;  
    A a;  
    a=get_A(t);  
    cout << "this is main() " << &t << endl;  
}
```

```txt
this is construction
this is construction
this is get_A() 0xa269fff77e
this is copy-construction
this is copy-operator
this is de-construction
this is main() 0xa269fff77e
this is de-construction
this is de-construction
------------------------
construction times:2
copy-construction times:1
copy-operator times:1
de-construction times:3

```

因为此时对象实例a调用了赋值运算函数，发生了copy-operator

---
```cpp
A get_A()
{
    A a;
    cout<<"this is get_A() "<<&a<<endl;
    return a;
}

int main() 
{
    {
        A a=get_A();
        cout << "this is main() " << &a << endl;
    }
   
    cout<<"------------------------\n";
    cout<<"construction times:"<<con<<endl;
    cout<<"copy-construction times:"<<cpy<<endl;
    cout<<"copy-operator times:"<<ope<<endl;
    //cout<<"move-operator times:"<<m_ope<<endl;
    cout<<"de-construction times:"<<decon<<endl;
    return 0; 
}
```

```txt
this is construction
this is get_A() 0x5145bff9af
this is copy-construction
this is de-construction
this is main() 0x5145bff9ff
this is de-construction
------------------------
construction times:1
copy-construction times:1
copy-operator times:0
de-construction times:2


```
```txt
A a=get_A();
与
get_A();
输出结果无区别
```




## 返回引用

```cpp
{  
    A a;  
    A &b=a;  
    A c=b;  
}
```

```txt
this is construction
this is de-construction
------------------------
construction times:1
copy-construction times:0
copy-operator times:0
de-construction times:1
```


## 指向类成员的指针

在 C++语言中，**可以定义一个指针，使其指向类成员或成员函数，然后通过指针来 访问类的成员**。这包括指向属性成员的指针和指向成员函数的指针

### 指向数据成员的指针

```txt
<数据类型><类名>::*<指针名>[=&<类名>::<非静态数据成员>]
```

由于类不是运行时 存在的对象。因此，在使用这类指针时，需要首先指定类的一个对 象，然后，通过对象来引用指针所指向的成员。

```txt
<类对象名>.*<指向非静态数据成员的指针> 
<类对象指针>->*<指向非静态数据成员的指针>
```

```cpp
int Node::*pa=&Node::a;  
Node n1{10,20,30};  
cout<<n1.*pa<<endl;
```

### 指向成员函数的指针

定义一个指向非静态成员函数的指针必须在三个方面与其指向的成员函数保持一致：参数 列表要相同、返回类型要相同、所属的类型要相同

```txt
<数据类型>(<类名>::*<指针名>)(<参数列表>)[=&<类名>::<非静态成员函数>]
```


由于类不是运行时存在的对象。因此，在使用这类指针时，需要首先指定类的一个对象， 然后，通过对象来引用指针所指向的成员。

```txt
(<类对象名>.*<指向非静态成员函数的指针>)(<参数列表>)
(<类对象指针>->*<指向非静态成员函数的指针>)(<参数列表>)
```


```cpp
void (Node::*sh)()=&Node::show;  
(n1.*sh)();
```

## 友元

只能出现在类定义中，因为友元不是授权类的成员，所以它不受其所在类的声明区域public private 和protect的影响

友元关系不能被继承，且不具有传递性

## 重载

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240511223904.png)

重载运算符的运算中至少有一个操作数是自定义类




# 继承 Inherit& Derive

类的继承，是新的类从已有类那里得到已有的特性
父类的私有属性会被继承并且占有空间

public（公有继承）：继承时保持基类中各成员属性不变，并且基类中private成员被隐藏。派生类的成员只能访问基类中的public/protected成员，而不能访问private成员；派生类的对象只能访问基类中的public成员。  
private（私有继承）：继承时基类中各成员属性均变为private，并且基类中private成员被隐藏。派生类的成员也只能访问基类中的public/protected成员，而不能访问private成员；派生类的对象不能访问基类中的任何的成员。  
protected（保护性继承）：继承时基类中各成员属性均变为protected，并且基类中private成员被隐藏。派生类的成员只能访问基类中的public/protected成员，而不能访问private成员；派生类的对象不能访问基类中的任何的成员。


public 作用：传承接口 间接的传承了数据(protected)
protected 作用：传承数据，间接封杀了对外接口。
private 统杀了数据和接口




### 派生类的构造(现代c++核心特性解析chap13 继承构造)

派生类构造函数执行的次序:
- 调用基类构造函数，调用顺序按照它们被继承时声明的顺序（从左到右）
- 调用内嵌成员对象的构造函数，调用顺序按照它们在类中声明的顺序
- 派生类的构造函数体中的内容

### 拷贝构造

```txt
派生类::派生类(const 派生类& another) :基类(another),派生类新成员(another.新成员) { }
```
**派生类中的默认拷贝构造器会调用父类中默认或自实现拷贝构造器**，若派生类中自实现拷 贝构造器,则必须**显示的调用**父类的拷贝构造器


```cpp
class A
{
public:
    int a=1;
    A() = default;
    A(int a_, int b_, int c_) :a(a_), b(b_), c(c_) {};
    A(const A&) { cout << "this is A\n"; };
protected:
    int b=2;
private:
    int c=3;
};
class B : protected A
{
public:
    B() = default;
    //B(int a_, int b_, int c_,int d_) :A(a_,b_,c_),d(d_) {};
    using A::A;
    B(int d_) :d(d_) {};
    void change()
    {
        a = 20;
        b = 20;
        //c = 20;
    }
    B(const B& another) :A(another), d(another.d) { cout << "this is B\n"; };
    void show()
    {
        cout << a << " " << b <<  " " << d << endl;
    }
    int d = 20;
};
int main()
{
    B b(1,2,3);
    B c(b);

    return 0;
}
```
```txt
this is A
this is B
```

因为显式调用了父类的拷贝构造函数

### 运算符重载

```txt
子类& 子类::operator=(const 子类& another)
{ 
    if(this == &another) return *this; //防止自赋值 
    父类::operator =(another); // 调用父类的赋值运算符重载 
    this->salary = another.salary;//子类成员初始化
    return * this; 
}
```
和派生类拷贝构造差不多，如果子类不重写父类运算符重载，会默认调用子类的

```cpp
//class A:
 A& operator=(const A&) { cout << "this is A operator=\n"; return *this; };
//class B
B& operator=(const B&v)
{
    A::operator=(v);
    cout << "this is B operator=\n"; 
    return *this; 
};
```

```txt
this is A operator=
this is B operator=
```

### 派生类成员的标识和访问

如果某派生类的多个基类拥有同名的成员，同时，派生类又新增这样的同名成员，在这 种情况下，派生类成员将 shadow(隐藏)所有基类的同名成员。这就需要使用作用域分辨符才能 调用基类的同名成员。


### 虚继承

```cpp
class A
{
public:
    int a ;
    A() = default;
    A(int a_) :a(a_) {};
};

class B :public A
{
public:
    using A::A;
};

class C :public A
{
public:
    using A::A;
};

class D :public B, public C
{
public:
    using B::B;
    using C::C;
};
int main()
{
    D d(10);
    return 0;
}
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240512101823.png)

钻石继承会报错

解决方法：虚继承
```cpp
class B :virtual public A
class C :virtual public A;
class D :public B, public C;
```

c++提供了，**虚基类和虚继承机制**，实现了**在多继承中只保留一份共同成员**。 虚基类，需要设计和抽象，虚继承，是一种继承的扩展。

在上面例子中，A类为虚基类，BC继承方式为虚继承



```cpp
 class D :public B, public C
 {
 public:
     using B::B;
     using C::C;
     void show()
     {
         cout <<"A::a:" << A::a << endl;
         cout <<"B::a:" << B::a << endl;
         cout <<"C::a:" << C::a << endl;
         cout <<"D::a:" << D::a << endl;
     }
 };
```
```txt
A::a:10
B::a:10
C::a:10
D::a:10
```


# 多态 PolyMorphism

同一消息为不同对象接受时可产生完全不同的行为

一个方法只有一个名字 但可以有多种形态，也就是程序中可以定义多个同名方法

如在 winows 环境下，用鼠标双击一个对象（这就是向对象传递一个消息），如果对象 是一个可执行文件，则会执行此程序，如果对象是一个文本文件，由会启动文本编辑器并打开 该文件。

### 赋值兼容

**赋值兼容规则是指在需要基类对象的任何地方都可以使用公有派生类的对象来替代**。赋值兼 容是一种默认行为，不需要任何的显示的转化步骤

- 派生类的对象可以赋值给**基类对象**。
- 派生类的对象可以初始化**基类的引用**。
- 派生类对象的地址可以赋给指向**基类的指针**

在替代之后，派生类对象就可以作为基类的对象使用，但只能使用从基类继承的成员。

父类也可以通过强转的方式转化为子类。 父类对象强转为子类对象后，访问从父类继承 下来的部分是可以的，但访问子类的部分，则会发生越界的风险


### 多态的形成条件

函数重载也是一种多态现象，通过命名倾轧在编译阶段决定，称为静态多态

动态多态，不是在编译阶段决定，而是在运行阶段决定
- 父类有**虚函数**
- 子类override**重写父类的虚函数**
- 通过已被**子类对象赋值的父类指针或引用**，调用共同接口


函数纯虚函数的类称为抽象基类
```cpp
virtual void fun()=0
```

- 含有纯虚函数的类，称为抽象基类，不可实列化。即不能创建对象，存在的意义就是被 继承，提供族类的公共接口，java 中称为 interface。 
- **纯虚函数只有声明，没有实现**，被“初始化”为 0。 
- **如果一个类中声明了纯虚函数，而在派生类中没有对该函数定义，则该虚函数在派生类 中仍然为纯虚函数，派生类仍然为纯虚基类**


***含有虚函数的类，析构函数也应该声明为虚函数**。**在 delete 父类指针的时候，会调用子 类的析构函数，实现完整析构***
```cpp
Animal * pa = new Dog; 
pa->voice();
delete pa;
```


- 只有类的成员函数才能声明为虚函数
- 静态成员函数不能是虚函数
- 内联函数不能是虚函数
- 构造函数不能是虚函数,因为发生构造时对象还不存在
- 析构函数可以是虚函数且通常声明为虚函数。


**依赖倒置原则的核心就是要我们面向接口编程，理解了面向接口编程，也就理解了依赖倒 置**

## 多态实现


C++的多态是通过一张**虚函数表**（Virtual Table）来实现的，简称为 V-Table。在这个 表中，主是要一个**类的虚函数的地址表**，这张表解决了继承、覆写的问题，保证其真实反应 实际的函数。这样，在**有虚函数的类的实例中这个表被分配在了这个实例的内存中**，所以， 当我们用父类的指针来操作一个子类的时候，这张虚函数表就显得由为重要了，它就像一个 地图一样，指明了实际所应该调用的函数。


C++的编译器应该是保证虚函数表的指针存在于对 象实例中最前面的位置（这是为了保证取到虚函数表的有最高的性能——如果有多层继承或 是多重继承的情况下）。 这意味着我们通过对象实例的地址得到这张虚函数表，然后就可以 遍历其中函数指针，并调用相应的函数



# 模版 Templates

泛型(Generic Programming)即是指具有**在多种数据类型上皆可操作**的含意。

泛型编程 的代表作品 STL 是一种高效、泛型、可交互操作的软件组件。
泛型编程最初诞生于 C++中，目的是为了实现 C++的 STL（标准模板库）。其语言支 持机制就是模板（Templates）。模板的精神其实很简单：**参数化类型**。换句话说，把一 个原本特定于某个类型的算法或类当中的类型信息抽掉，抽出来做成模板参数 T。


# IO类图

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240512141511.png)


