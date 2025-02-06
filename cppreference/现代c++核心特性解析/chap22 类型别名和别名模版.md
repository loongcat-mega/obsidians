
## 类型别名

typedef可以实现

c++11引入的using也可以实现
```cpp
using lli = long long int;
```
## 别名模版

它的实例化过程是用自己的模板参数替换原始模板的模板 参数，并实例化原始模板
```cpp
template<class T> 
using int_map = map<int, T>;
```

别名与模版结合

