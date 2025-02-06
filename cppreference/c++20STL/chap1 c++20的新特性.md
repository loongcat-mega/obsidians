# format(format头文件中)

format函数可以接受一个string_view格式的字符串和一个可变参数参数包，并返回一个字符串

```cpp
template<typename... Args> 
string format(string_view fmt, const Args&... args);
```

格式化字符串使用大括号作为类型安全的占位符，可以将任何兼容类型的值转换为合理的字符串表示形式，可以包含多个占位符，并且可以指定顺序，也可以指定对齐方式

```cpp
    int main()
    {
        //使用{}作为占位符
        cout << format("hello {}","world") << endl;
        //包含多个占位符
        cout << format("{},{}","hello","world") << endl;
        //指定占位符顺序
        cout << format("{1},{0}","hello","world") << endl;
        int  a = 10;
        cout << format("a的值为{}", a) << endl;

        //指定精度对齐
        //空格为分隔符居中对齐
        cout << format("a的值为{: ^5}", a) << endl;
        //!为分隔符左对齐
        cout << format("a的值为{:!<5}", a) << endl;
        //?为分隔符右对齐
        cout << format("a的值为{:?>5}", a) << endl;
        double pi = 3.14159265358979323;
        //设置十进制数组的精度
        cout << format("pi的值为{:.5}", pi) << endl;

        return 0;
    }
```
```txt
hello world
hello,world
world,hello
a的值为10
a的值为 10
a的值为10!!!
a的值为???10
pi的值为3.1416
```
# constexpr


在 C++11 及以后的版本中，`constexpr` 修饰符用于声明在编译时就可以计算出结果的表达式。**这些表达式必须完全由字面量类型组成**

分配给STL容器的对象内存，在编译时分配，也在编译时释放

除了 `std::array`，C++标准库中还有一些其他的容器类型可以被视为字面量类型:

1.  **`std::tuple`**:
    -   `std::tuple` 是一种可以在编译时初始化的容器类型,可以使用 `constexpr` 进行操作。
2.  **`std::pair`**:
    -   `std::pair` 是一种可以在编译时初始化的容器类型,可以使用 `constexpr` 进行操作。
3.  **`std::variant`**:
    -   `std::variant` 是一种可以在编译时初始化的联合类型,可以使用 `constexpr` 进行操作。
4.  **`std::bitset`**:
    -   `std::bitset` 是一种可以在编译时初始化的定长位序列容器,可以使用 `constexpr` 进行操作。
5.  **`std::string_view`**:
    -   `std::string_view` 是一种可以在编译时初始化的字符串视图类型,可以使用 `constexpr` 进行操作。





# 三向比较符


# 概念(concept)和约束(constraint)

require关键字是c++20的新特性，将约束应用于模版

# 视图

