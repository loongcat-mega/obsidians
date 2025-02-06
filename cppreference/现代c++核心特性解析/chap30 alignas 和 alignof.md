C++11中新增了alignof和alignas两个关键字，其中alignof运 算符可以用于获取类型的对齐字节长度，alignas说明符可以用来改变 类型的默认对齐字节长度。这两个关键字的出现解决了长期以来C++标 准中无法对数据对齐进行处理的问题。

通常来说对齐长度和CPU访问数据总线的宽度有关系，比如CPU访问32位宽度的数据总线，就会期待数据是按照32位对齐，也就是四字节，这样CPU读取4字节的数据只需要对总线访问一次


## alignof

alignof必须是针对类型的
```cpp
int a = 0; 
auto x3 = alignof(decltype(a));
```



## alignas

该说明符可以接受类型或者 常量表达式。特别需要注意的是，该常量表达式计算的结果**必须是一个 2的幂值**，否则是无法通过编译的。


它既可 以用于结构体，也可以用于结构体的成员变量。**如果将alignas用于结 构体类型，那么该结构体整体就会以alignas声明的对齐字节长度进行 对齐**

如果修改结 构体成员的对齐字节长度，那么结构体本身的对齐字节长度也会发生变 化，因为**结构体类型的对齐字节长度总是需要大于或者等于其成员变量 类型的对齐字节长度**

```cpp
class alignas(8) A  
{  
public:  
    alignas(2) char c;  
    alignas(2) char d;  
    alignas(2) char e;  
    alignas(4) char f;  
    //alignas(4) int a;  
};
cout<<"alignof(A)= "<<alignof(A)<<endl;  
cout<<"sizeof (A)= "<<sizeof (A) <<endl;
```

```text
alignof(A)= 8
sizeof (A)= 16
```

如果把A中alignas(4)改为2，那么sizeof(A)=8

**结构体类型的对齐字节长 度，并不能影响声明变量时变量的对齐字节长度**

