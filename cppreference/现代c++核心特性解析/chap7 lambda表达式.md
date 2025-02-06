
c++11引入
c++14新增广义捕获


## lambda表达式语法
```c++
[ captures ] ( params ) specifiers exception -> ret { body }
[捕获列表](函数参数){函数体};
[]{}不能缺省
()可以缺省
```
\[ captures \]，可以捕获当前函数作用域的零个或多个变量，变量之间用逗号分隔

( params ),可选参数列表，在不需要参数的时候可以忽略参数列表，如果存在说明符，则形参列表不能省略

sepecifiers,可选限定符，在c++11中可以用mutable，允许在lambda表达式函数体内改变按值捕获的变量

exception，可选异常说明符，可以用noexpect来指明lambda是否会抛出异常

ret，可选返回值类型，返回类型后置





## 捕获列表


```c++
[this] 捕获this指针
[=]捕获lambda表达式定义作用域的全部变量的值
[&]捕获lambda表达式定义作用域的全部变量的引用

```




捕获列表中变量存在于两个作用域，lambda表达式定义的函数作用域，与lambda表达式函数体的作用域。前者为了捕获变量，后者为了使用变量
捕获的变量必须是一个自动存储类型，即非静态局部变量

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240430114211.png)
以上代码无法通过编译，因为捕获了静态变量与全局变量

进一步来说，如果我们将一个lambda表达式定义在全局作用域， 那么lambda表达式的捕获列表必须为空

### 捕获值和捕获引用

捕获值实景函数作用域的捕获值复制到lambda表达式对象的内部，成为lambda表达式的成员变量

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240430115006.png)


**捕获值，捕获的变量不能修改，因为operator()默认是const的**，加mutable可以修改,但再次访问会继承上次修改的值，若将Lambda表达式看成仿函数，则捕获值相当于类的成员变量
```c++
int x=0;
auto foo=[x]()mutable{x++;};
foo();//x=1
foo();//x=2
foo();//x=3
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240428212737.png)


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240428212806.png)



```cpp
int a=10;  
auto p= [=](){};  
cout<<sizeof p<<endl;  //1
return 0;
```
展开后：
```cpp
class __lambda_9_12
{
public: 
inline void operator()() const
{
}

// inline /*constexpr */ __lambda_9_12(__lambda_9_12 &&) noexcept = default;

};
```
如果捕获列表的值未被lambda表达式使用，则不会成为类成员

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240429104809.png)


如果变量是const而非volatile 的整形或枚举型，并且已经用常量表达式初始化，或者是constexpr且没有mutable成员，则不需要捕获


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240429105107.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240429105316.png)



捕获引用可以修改，而不需要解引用
```c++
auto foo=[&x]{x++;};
```

## lambda实现原理

lambda表达式在编译期会由编译器自动生成一个闭包类，在运行时 由这个闭包类产生一个对象，我们称它为闭包。在C++中，所谓的闭包 可以简单地理解为一个匿名且可以包含定义时作用域上下文的函数象。

## STL中使用lambda表达式

sort find_if等

## 广义捕获

C++14标准中定义了广义捕获，所谓广义捕获实际上是两种捕获方 式，第一种称为简单捕获，这种捕获就是我们在前文中提到的捕获方 法。第二种叫作**初始化捕获**， 这种捕获方式是在C++14标准中引入的，它解决了简单捕获的一个重要 问题，即只能捕获lambda表达式定义上下文的变量，而无法捕获表达式 结果以及自定义捕获变量名

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240430115529.png)

应用场景：使用移动操作减少代 码运行的开销
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240430115559.png)

上面这段代码使用std::move对捕获列表变量x进行初始化，这样避 免了简单捕获的复制对象操作，代码运行效率得到了提升。

## 泛型lambda表达式

使用auto占位符作为形参

## 模版lambda表达式

C++委员会决定在C++20中添加模板对lambda 的支持
```text
[]<typename T>(T t) {}
```

## 总结

它很好地解决 了过去C++中无法直接编写内嵌函数的尴尬
