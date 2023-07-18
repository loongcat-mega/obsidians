
#### 拷贝range中的元素时，使用
```c++
for(auto i:range)
```
利用i遍历并获得range容器中每个值，但是i无法影响到range容器中的元素

#### 修改range中的元素时，使用
```c++
for(auto &&i:range)
```
可以对容器中的内容进行赋值，可通过对i赋值来做到容器range的填充

#### 只读range中的元素时，使用
```c++
for(condt auto &x:range)
```

`&&`右值引用
返回值为引用：左值引用