
具体来说，是指一个表达式中的子表达式的求 值顺序，而这个顺序在C++17之前是没有具体说明的，所以编译器可以 以任何顺序对子表达式进行求值。比如说foo(a, b, c)，这里的 foo、a、b和c的求值顺序是没有确定的

从C++17开始，**函数表达式一定会在函数的参数之前求值**
也就是 说在foo(a, b, c)中，foo一定会在a、b和c之前求值。但是请注意， 参数之间的求值顺序依然没有确定

```cpp
#include<iostream>
using namespace std;
int fa(int a)
{
    cout<<"fa"<<endl;
    return a;
}
int fb(int a)
{
    cout<<"fb"<<endl;
    
    return a;
}
int fc(int a)
{
    cout<<"fc"<<endl;
    
    return a;
}
int get_sum(int a,int b,int c)
{
    cout<<"sum"<<endl;
    
    return a+b+c;
}
int main()
{
    int a=get_sum(fa(1),fb(2),fc(3));
    return 0;
}
```
输出 fc fb fa sum

对于后缀表达式和移位操作符而言，表达式求值总是从左往右
而对于赋值表 达式，这个顺序又正好相反，它的表达式求值总是从右往左

