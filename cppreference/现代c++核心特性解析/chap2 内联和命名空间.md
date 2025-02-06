
大型工程中，同名函数和类型会造成编译冲突问题，为了解决此问题程序员可以将函数和类型纳入命名空间中，这样在不同命名空间的函数和类型就不会产生冲突，当要使用它们的时候只需打开其指定的命名空间即可

```cpp
//  
// Created by dlut2102 on 2024/4/29.  
//  
#include<iostream>  
using namespace std;  
namespace s1  
{  
    void foo()  
    {  
        cout<<"this is namesapce s1\n";  
    }  
}  
namespace s2  
{  
    void foo()  
    {  
        cout<<"this is namesapce s2\n";  
    }  
}  
using namespace s1;  
//using namespace s2;  
int main()  
{  
    foo();  
    s1::foo();  
    s2::foo();  
    return 0;  
}

this is namesapce s1
this is namesapce s1
this is namesapce s2

```
C++11标准增强了命名空间的特性，提出了内联命名空间的概念。内联命名空间能够把空间内函数和类型导出到父命名空间中，这样即使不指定子命名空间也可以使用其空间内的函数和类型了

利用这个特性，可以无缝维护升级代码

```cpp
namespace s1  
{  
    inline namespace chv2  
    {  
        void foo()  
        {  
            cout<<"version 1.0.1\n";  
        }  
    }  
    namespace ch  
    {  
        void foo()  
        {  
            cout<<"version 1.0.0\n";  
        }  
    }  
}
```
客户端代码不需要更改

`s1::foo()`

该特性可以帮助 库作者无缝切换代码版本而无须库的使用者参与。另外，使用新的嵌套 命名空间语法能够有效消除代码冗余，提高代码的可读性。