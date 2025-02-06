字面量（literal）是用于表达源代码中一个固定值的表示法（notation）
C++自带四种字面量：
- 整形 对于一个没有特殊后缀的整形变量，默认为int
- 浮点型
- 字符
- 字符串

## 十六进制浮点字面量

hexfloat可以将浮点数格式化为十六进制的字符串
defaultfloat可以将格式还原到十进制

```cpp
#include <iostream> 
int main() 
{ 
    double float_array[]{ 5.875, 1000, 0.117 }; 
    for (auto elem : float_array) 
    { 
        std::cout << std::hexfloat << elem << " = " 
        << std::defaultfloat << elem << std::endl;
    } 
}

0xb.cp-1 = 5.875
0xf.ap+6 = 1000
0xe.f9db22d0e5608p-7 = 0.117
```

虽然C++11已经具备了在输入输出的时候将浮点数格式化为十六进 制的能力，但遗憾的是我们并不能在源代码中使用十六进制浮点字面量 来表示一个浮点数。幸运的是，这个问题在C++17标准中得到了解决

总之，我们在C++17中可以根据实际需求选择浮点数的表示 方法，当需要精确表示某个浮点数的时候可以采用十六进制浮点字面 量，其他情况使用十进制浮点字面量即可


## 二进制整数字面量

在C++14标准中定义了二进制整数字面量，正如十六进制（0x， 0X）和八进制（0）都有固定前缀一样，二进制整数字面量也有前缀0b 和0B。

## 单引号作为整数分隔符

```cpp
int a=0b11'111;
```

## 原生字符串字面量


C++11标 准引入原生字符串字面量的概念

声明原生字符串字面量
```text
prefix R"delimiter(raw_　characters)delimiter"
````

```cpp
std::string s=R"(dsfdsfds['f\ffds\f]\sd]f\]sd\f]sd\a)";
```

delimiter可以是由除括号、反斜杠和空格以外的任何源字符构成 的字符序列，长度至多为16个字符。通过添加delimiter可以改变编译 器对原生字符串字面量范围的判定，从而顺利编译带有)"的字符串

## 用户定义字面量

在C++11标准中新引入了一个用户自定义字面量的概念，程序员可 以通过自定义后缀将整数、浮点数、字符和字符串转化为特定的对象。 这个特性往往用在需要大量声明某个类型对象的场景中，它能够减少一 些重复类型的书写，避免代码冗余。一个典型的例子就是不同单位对象 的互相操作

对于如下定义：
```cpp
double len=10.2;
```
此时len是单位是什么？

如果能写成下文就好了：
```cpp
double d1=10_cm;
double d2=10_mm;
```
为此，c++11允许用户自定义字面量，但是UDL必须以下划线开头，但不能是双下划线


[用户定义字面量](https://zh.cppreference.com/w/cpp/language/user_literal#Popover19-toggle:~:text=%E7%94%A8%E6%88%B7%E5%AE%9A%E4%B9%89%E5%AD%97%E9%9D%A2%E9%87%8F)

```cpp
#include<iostream>
using namespace std;
void operator""_r(const char *str,size_t len)
{
    cout<<str<<" "<<len<<endl;
}

#define std_meter 1;
long double  operator""_dm(long double d)
{
    return d/10*std_meter;
}
long double  operator""_cm(long double d)
{
    return d/100*std_meter;
}
int main()
{
    "abc"_r;
    cout<<0.5_dm/0.5_cm<<endl;
}

```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240428101959.png)

甚至可以这样，进制转换直接写成后缀：
```cpp
#include<iostream>
using namespace std;
int operator""_bin(const char *str,size_t len)
{
   int ans=0;
   for(int i=0;i<len;i++)
   {
        ans=(ans<<1)+str[i]-'0';
   }
   return ans;

}

#define std_meter 1;
long double  operator""_dm(long double d)
{
    return d/10*std_meter;
}
long double  operator""_cm(long double d)
{
    return d/100*std_meter;
}
int main()
{
    cout<<"10100"_bin<<endl;
    cout<<0.5_dm/0.5_cm<<endl;
}

```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240428102709.png)

但是要注意UDL形参列表有限制条件

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240428102934.png)

像下面这种做法就是错误的
```cpp
long double d1=0.5;
cout<<d1_dm/0.5_cm<<endl;
```
因为此时d1是变量而不是字面量


值得注意的是在C++11的标准中，双引号和紧跟的标识符中间必须 有空格，不过这个规则在C++14标准中被去除
在C++14标准中，标识 符不但可以紧跟在双引号后，而且还能使用C++的保留字作为标识符

## 总结

二进制整数 字面量和十六进制浮点字面量增强了字面量的表达能力，让单引号作为 整数的分隔符优化了长整数的可读性，用户自定义字面量让代码库作者 能够为客户提供更加简洁的调用对象的方法，最实用的应该要数原生字 符串字面量了，它让我们摆脱了复杂字符串中转义字符的干扰，让字符 串所见即所得，在类似代码或者正则表达式等字符串上十分有用

