`a <=> b`可以产生三种结果，该结果只能与0比较，分别为大于0，小于0，等于0

默认情况下自定义类型是不存在三 向比较运算符函数的，需要用户显式默认声明，比如在结构体B和D中声 明auto operator <=> (const B&) **const** = default


作用是类与类之间比较的时候，不用重载各种运算符，只需要重载`<=>`与`==`两个运算符就可以，编译器自动重载大于小于等运算符，`!=`不需要重载，编译器可以根据`==`自动按照需要重载该运算符

```cpp
    class A
    {
    public:
        int x;
        int y;
        A() = default;
        A(int val_,int y_) :x(val_),y(y_) {};
        auto operator<=> (const A& a)const
        {
            if (auto cmp = (this->x <=> a.x); cmp != 0)
                return cmp;
            if (auto cmp = (this->y <=> a.y); cmp != 0)
                return cmp;
            return (this->y <=> a.y);
        }
        bool operator==(const A& a)const
        {
            auto cmp = this->x <=> a.x;
            if (cmp == 0)
                return (y <=> a.y)==0;
            return false;

        }
    };
```

当类内有多个成员属性时，逐一利用`<=>`比较类属性

