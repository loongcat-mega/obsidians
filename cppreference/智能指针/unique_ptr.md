
std::unique_ptr 是一种智能指针，它通过指针持有并管理另一对象，并在 unique_ptr 离开作用域时释放该对象。
在发生下列两者之一时，用关联的删除器释放对象：

    管理它的 unique_ptr 对象被销毁。
    通过 operator= 或 reset() 赋值另一指针给管理它的 unique_ptr 对象。 

对象的释放是通过调用 get_deleter()(ptr)，用可能是由用户提供的删除器进行的。默认删除器使用 delete 运算符，它销毁对象并解分配内存。

unique_ptr 亦可以不占有对象，该情况下称它为空 (empty)。

std::unique_ptr 有两个版本：

    管理单个对象（例如以 new 分配）
    管理动态分配的对象数组（例如以 new[] 分配） 

此类满足可移动构造 (MoveConstructible) 和可移动赋值 (MoveAssignable) ，但不满足可复制构造 (CopyConstructible) 或可复制赋值 (CopyAssignable) 。 



在任何给定的时刻，只能有一个指针管理内存.当指针超出作用域时，内存将自动释放
不可以copy，只可以move
三种创建方式
| 创建方式        | 演示                                               | 特点 |     |
| --------------- | -------------------------------------------------- | ---- | --- |
| 通过已有裸指针  | `Cat *c=new Cat("hello"); unique_ptr<Cat> uptr{c}` | 会使得两个指针指向同一块内存地址，不满足独占的特点。     |     |
| 通过new         | `unique_ptr<Cat>uptr1{new Cat("ddd")};`            |      |     |
| 通过make_unique | `unique_ptr<Cat>uptr2= make_unique<Cat>("456");`   |      |     |


构造函数
```cpp
 // Constructors.

/// Default constructor, creates a unique_ptr that owns nothing.
template<typename _Del = _Dp, typename = _DeleterConstraint<_Del>>
constexpr unique_ptr() noexcept
: _M_t()
{ }

/** Takes ownership of a pointer.
*
* @param __p  A pointer to an object of @c element_type
*
* The deleter will be value-initialized.
*/
template<typename _Del = _Dp, typename = _DeleterConstraint<_Del>>
explicit
unique_ptr(pointer __p) noexcept
: _M_t(__p)
{ }

/// Creates a unique_ptr that owns nothing.  
template<typename _Del = _Dp, typename = _DeleterConstraint<_Del>>  
constexpr unique_ptr(nullptr_t) noexcept  
: _M_t()  
{ }
```








可以通过unique_ptr的成员函数get来获取指针地址

```cpp
// Disable copy from lvalue.  
unique_ptr(const unique_ptr&) = delete;  
unique_ptr& operator=(const unique_ptr&) = delete;
```
删除了拷贝构造和拷贝赋值函数

```cpp
 operator*() const  
 {  
__glibcxx_assert(get() != pointer());  
return *get();  
 }
  /// Return the stored pointer.
  pointer
  operator->() const noexcept
  {
_GLIBCXX_DEBUG_PEDASSERT(get() != pointer());
return get();
  }

  /// Return the stored pointer.
  pointer
  get() const noexcept
  { return _M_t._M_ptr(); }
```

重载了`->`运算符，返回值是`get()`

重载了`*`运算符，返回值是`*get()`


```cpp
 Cat *c=new Cat("hello");  

unique_ptr<Cat>uptr1{new Cat("ddd")};  

uptr1= nullptr;  
uptr1.reset(c);
```

```txt
hello : Individual Construction
ddd : Individual Construction
ddd : Deconstruction
hello : Deconstruction
```
```cpp
void
reset(pointer __p = pointer()) noexcept
{
static_assert(__is_invocable<deleter_type&, pointer>::value,
      "unique_ptr's deleter must be invocable with a pointer");
_M_t.reset(std::move(__p));
}
```


```cpp
void reset(pointer __p) noexcept  
{  
const pointer __old_p = _M_ptr();  
_M_ptr() = __p;  
if (__old_p)  
_M_deleter()(__old_p);  
}

pointer release() noexcept
{
pointer __p = _M_ptr();
_M_ptr() = nullptr;
return __p;
}
```



release：控制权交回裸指针，并且不会自动调用析构函数
reset:如果原来类内指针不为空，则析构，并且重新设置指针


