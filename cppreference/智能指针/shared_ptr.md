共享指针

```txt
A smart pointer with reference-counted copy semantics.
A `shared_ptr` object is either empty or _owns_ a pointer passed to the constructor. Copies of a `shared_ptr` share ownership of the same pointer. When the last `shared_ptr` that owns the pointer is destroyed or reset, the owned pointer is freed (either by `delete` or by invoking a custom deleter that was passed to the constructor). A `shared_ptr` also stores another pointer, which is usually (but not always) the same pointer as it owns. The stored pointer can be retrieved by calling the `get()` member function. The equality and relational operators for `shared_ptr` only compare the stored pointer returned by `get()`, not the owned pointer. To test whether two `shared_ptr` objects share ownership of the same pointer see `std::shared_ptr::owner_before` and `std::owner_less`.
```

std::shared_ptr 是一种通过指针保持对象共享所有权的智能指针。**多个 shared_ptr 对象可持有同一对象**。下列情况之一出现时销毁对象并解分配其内存：

    最后剩下的持有对象的 shared_ptr 被销毁；
    最后剩下的持有对象的 shared_ptr 被通过 operator= 或 reset() 赋值为另一指针。 

用 delete 表达式或在构造期间提供给 shared_ptr 的定制删除器销毁对象。

shared_ptr 能在存储指向一个对象的指针时共享另一对象的所有权。此特性能用于在持有其所属对象时，指向成员对象。存储的指针可以使用 get()、解引用或比较运算符访问。被管理指针在使用计数抵达零时传递给删除器。

shared_ptr 也可不持有对象，该情况下称它为空 (empty)（若以别名使用构造函数创建，空 shared_ptr 可拥有非空的存储指针）。

shared_ptr 的所有特化都满足可复制构造 (CopyConstructible) 、可复制赋值 (CopyAssignable) 和可小于比较 (LessThanComparable) 的要求，并可按语境转换为 bool。

多个线程能在不同的 shared_ptr 对象上调用所有成员函数（包含复制构造函数与复制赋值）而不附加同步，即使这些实例是同一对象的副本且共享所有权也是如此。若多个执行线程访问同一 shared_ptr 对象而不同步，且任一线程使用 shared_ptr 的非 const 成员函数，则将出现数据竞争；std::atomic<shared_ptr> 能用于避免数据竞争。 

```cpp
shared_ptr<Cat> get_Cat(shared_ptr<Cat> p)
{
    (*p).info();
    return p;
}

void main()
{
    shared_ptr<int> sptr=make_shared<int>(8);
    shared_ptr<Cat> sptr1= make_shared<Cat>("mmmm");
    shared_ptr<Cat> sptr2=sptr1;
    shared_ptr<Cat> sptr3= get_Cat(sptr1);
    cout<<sptr1.use_count()<<" "<<sptr2.use_count()<<" "<<sptr3.use_count()<<endl;
}
```

```txt
mmmm : Individual Construction
this is mmmm
3 3 3
mmmm : Deconstruction
```

使用make_shared会维护弱引用计数


