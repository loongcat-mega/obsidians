Coredump（核心存储）是进程异常终止或崩溃时的内存快照，操作系统会在程序发生异常而异常在进程内部又没有被捕获的情况下，会把进程此刻内存、寄存器状态、堆栈指针、内存管理信息以及函数调用堆栈等信息转储保存在一个文件里（Corefile）。Coredump 对于开发者诊断和调试程序是非常有帮助的，因为对于有些程序错误很难重现，例如偶发的指针越界，而 Corefile 可以再现程序出错时的情景

## coredump开启
```shell
# 查看当前 corefile 大小，为 0 则表示禁止产生 corefile
ulimit -c

# 设置无限大
ulimit -c unlimited

# 设置 corefile 大小 100blocks
ulimit -c 100 
```

## gdb调试

```shell
gdb [execfile] [corefile]
```
```shell 
# 打印函数调用堆栈，第一行即为发生 Core 的最后调用处
bt

# 切换指定的一帧
f {num} 

# 打印当前函数的指定变量值
print{variable}

# 打印当前函数的参数名和值
info args

# 打印当前函数中所有局部变量和值
info local

# 查看当前栈帧的汇编代码
disas
```


