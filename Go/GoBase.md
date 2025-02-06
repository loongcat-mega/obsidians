git 
## 关键字和标识符

导出标识符 

非导出标识符

## 基本类型和他们的字面量表示


基本内置类型

零值

基本类型的字面量形式

## 常量和变量

类型不确定值和类型确定值

类型不确定常量的显式类型转换

类型推断

具名常量

类型确定具名常量

常量声明中的自动补全

常量声明中使用iota

变量声明和赋值操作

标准变量声明

纯赋值

短变量声明

每个局部声明的变量至少要被有效使用一次

变量和常量作用域

## 运算操作符

常量表达式

关于溢出

运算符结果类型推断

op=

自增自减

字符串拼接+

## 函数声明和调用

函数声明：参数列表 返回值 return

函数调用

函数调用的退出阶段

匿名函数
```go
	t, p := func() (int, int) {
		return 10, 20
	}() //加上小括号表示立即调用
	_, _ = t, p
	fmt.Println(t, p)

	t, p = func(x, y int) (int, int) {
		return t + p, t - p
	}(t, p)

	fmt.Println(t, p)
	```

闭包函数

## 代码包和包引入


包引入

`go doc fmt`
`go doc fmt.Println`

一个代码包可以由若干Go源文件组成，一个代码包的源文件必须处于同一个目录下。一个目录下的所有源文件都必须处于同一个代码包中，即这些源文件开头的package pkgname语句必须一致。所以一个代码包对应着一个目录，一个代码包目录下的每个字目录对应的都是另外一个独立的代码包

init函数

初始化顺序

完整的引入声明语句形式

匿名引入

每个非匿名引入必须至少要被使用一次


## 表达式 语句 和简单语句


表达式表示一个值
一条语句表示一个操作



单值与多值表达式

## 基本 流程控制语法

if-else
```go
if InitSimpleStatement; Condition {
   // do something

} else {
   // do something

}
```

for
```go
for InitSimpleStatement; Condition; PostSimpleStatement {  // do something  }
```


for-range

```go
	for c := 1; c < 10; c++ {
		fmt.Println(c)
	}
	for i := range 10 {
		fmt.Println(i)

	}
	var ArrInt = [5]int{1, 2, 3, 4}
for i, ele := range ArrInt {
		fmt.Println(i, ele)

}
	```


switch-case

fallthrough


协程 延迟函数调用 恐慌和恢复 并发技术等属于广义上的流程控制语句


goto

包含跳转标签的break和continue语句


## 协程 延迟函数调用 以及恐慌和恢复


协程 goroutine
协程有时也被称为绿色线程，是由程序的运行时维护的线程，一个绿色线程的内存开销和情景转换时耗比一个系统线程常常要小得多，协程是Go程序内部唯一的并发实现方式

在函数调用之前使用一个go关键字，即可让此函数运行在一个新的协程之中。当函数调用退出结束后，这个新的协程也就随之退出，一个协程的所有返回值必须被全部舍弃


并发concurrent计算 

- 同时读写
- 同时写

可能会出现一个数据共享问题（数据竞争）


```go
var wg sync.WaitGroup

//注册一个新任务
	wg.Add(1)
	go SayHello(5)
	//阻塞在这，直到所有任务都已完成
	wg.Wait()

func SayHello(times int) {
	for i := range times {
		log.Println("hello")
		_ = i
	}
	wg.Done()
}
```


协程的状态：
运行态和阻塞态之间切换



延迟函数调用：
当一个延迟调用语句被执行时，并不会立即执行其函数语句，而是被压入栈中，当主调函数进入退出阶段后，才会依次弹出延迟调用函数并调用
一个延迟调用可以修改包含此延迟调用的最内层函数返回值


恐慌和恢复：
Go不支持异常抛出和捕获，而是推荐使用返回值显式返回错误，由和异常抛出/捕获类似的机制：恐慌/恢复机制

使用内置函数panic来产生一个恐慌来使当前协程进入恐慌状态，一旦一个函数调用产生一个恐慌，此函数调用将立即进入他的退出阶段，通过在一个延迟函数调用之中调用内置函数recover，当前协程中的一个恐慌可以被消除，从而使当前协程重新进入正常状态。
如果一个协程在恐慌状态下退出，它将使整个程序崩溃



## Go类型系统概述

- 指针类型`*T`
- 结构体类型
- 函数类型
- 容器类型
    - 数组类型 `[5]T`
    - 切片类型`[]T`
    - 字典类型`map[Tkey]Tval`
- 通道类型：同步并发的协程
- 接口类型：反射和多态


类型定义type

类型别名

具名类型和无名类型

## 指针

支持垃圾回收

## 结构体


结构体类型和结构体字面量表示形式

结构体字面量表示形式和结构体值的使用

组合字面量不可寻址但可被取地址

结构体值的比较

匿名结构体类型可以使用在结构体字段声明中

## 数组 切片和映射



容器字面量的表示形式

容器类型零值的字面量表示形式

查看容器值的长度和容量

读取和修改容器的元素

容器赋值：当一个切片赋值给另一个切片后，他们将共享底层的元素

添加和删除容器元素
```go
m[k]=e
delete(m,k)
```

使用make函数来创建切片和映射

使用new来创建容器值

从数组或切片派生切片

切片转数组指针

切片转数组

使用copy来复制切片元素

遍历容器元素

clear清除/ 重置容器元素

数组指针当成数组来使用
提高访问效率
```go
	for range &BigArr {
		num1++
	}
	fmt.Println(num1)
	for range BigArr[:] {
		num2++
	}
```


删除一段切片元素
```go
s = append (s[:from],s[to:]...)
```

```go
	DeleteArr := []int{1, 2, 3, 4, 5, 6}
	res := DeleteEle(DeleteArr, keep)
	for _, val := range res {
		fmt.Println(val)
	}

func DeleteEle(s []int, keep func(int) bool) []int {
	res := s[:0]
	for _, val := range s {
		if keep(val) {
			res = append(res, val)

		}
	}
	return res
}

func keep(val int) bool {
	return val%2 == 1
}

```


特殊的插入和删除
```txt
1| // 前弹出(pop front，又称shift) 
2| s, e = s[1:], s[0]  
3| // 后弹出(pop back)  
4| s, e = s[:len(s)-1], s[len(s)-1] 
5| // 前推(push front)
6| s = append([]T{e}, s...) 7| // 后推(push back)  
8| s = append(s, e)
```


## 字符串 

字符串和字节切片之间的转换



## 函数

函数签名和函数类型

变长参数和变长参数函数类型，变长参数总为一个切片类型

```go
func (vals ...int64)(sum int64)
```

函数原型

同一个包中声明的函数名称不能重复

所有函数调用的传参均属于值复制

有返回值的函数调用是一种表达式，但是有多个返回值的函数传参有要求

闭包函数

匿名函数
```go
	isMultipleofX := func(x int) func(int) bool {
		return func(n int) bool {
			return n%x == 0
		}

	}

	var isMultipleof5 = isMultipleofX(5)
	fmt.Println(isMultipleof5(15))
```



## 通道

通道的主要作用是用来实现并发同步
**不要让计算通过共享内存来通讯，而应该让他们通过通讯来共享内存**。当通过共享内存来通信的时候，我们应当使用一些传统的并发同步技术，如互斥锁等来避免数据竞争。通道可看作是在一个程序内部先进先出的数据队列，

```go
chan T //表示一个元素类型为T的双向通道类型，允许向通道收发数据

chan <- T //单向发送通道，不允许接收

<- chan T //单向接收通道，不允许发送

```

通道操作

```go
close(ch)//关闭通道
ch <- v //向通道发送一个值v

v = <- ch //从通道读取一个值

cap(ch) // 查询通道容量


len(ch) // 查询通道长度
```



```go
func CHANNEL() {
	var ball = make(chan string)
	kickBall := func(playerName string) {
		for {
			fmt.Print(<-ball, "传球", "\n")
			time.Sleep(time.Second)
			ball <- playerName
		}
	}

	go kickBall("张三")
	go kickBall("李四")
	go kickBall("六大")

	ball <- "裁判"
	var c chan bool
	<-c//
}
```

for-range用于通道，此循环将不断地尝试用一个通道接收数据，直到此通道关闭且他的缓冲队列为空为止，此循环变量只能有一个


## 方法


```go

type Book struct {
	pages int
}

func (b Book) getPage() int {
	return b.pages
}
func (b *Book) setPage(page int) {
	b.pages = page
}

func main() {
	b := Book{10}
	fmt.Println(b)
	b.setPage(24)
	fmt.Println(b)
	fmt.Println(b.getPage())

}
```

## 接口

接口类型介绍和类型集。接口类型是通过内嵌若干接口元素来定义类型条件的。

空接口类型的类型集包含了所有的非接口类型，所有类型均实现了空接口类型

值包裹

多态：接口值调用类型方法

反射：一个接口值中存储的动态类型信息可以被用来检视此接口值的动态值和操纵此动态值所引用的值，这称为反射








