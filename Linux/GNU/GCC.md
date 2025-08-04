## GCC
 GNU Complier Collection  是GNU开发的编程语言编译器

GCC 编译工具链toolchain ：
- gcc-core gcc编译器
- Binutils 包括链接器Id，汇编器as，目标文件格式查看器
- glibc 包含了主要的c语言标准库函数

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020231202104223.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020231202104336.png)




gcc编译分四个阶段：
- 预处理 
- 编译
- 汇编
- 链接

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231223144215.png)

```bash
gcc -E hello.c -o hello.i   对hello.c文件进行预处理，生成了hello.i 文件
gcc -S hello.i -o hello.s    对预处理文件进行编译，生成了汇编文件
gcc -c hello.s -o hello.o  对汇编文件进行编译，生成了目标文件
gcc hello.o -o hello 对目标文件进行链接，生成可执行文件
gcc hello.c -o hello 直接编译链接成可执行目标文件
gcc -c hello.c 或 gcc -c hello.c -o hello.o 编译生成可重定位目标文件
```

```bash
gcc 编译多源程序

gcc f1.c f2.c -o m.out
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231223144653.png)

### **1. `-U_FORTIFY_SOURCE`**​

- ​**作用**​：取消之前定义的 `_FORTIFY_SOURCE` 宏（如果有）。
- ​**背景**​：`_FORTIFY_SOURCE` 是 GCC 的一个安全增强功能，用于检测缓冲区溢出等内存安全问题。`-U` 表示取消定义。

---

### ​**2. `-fstack-protector`**​

- ​**作用**​：启用栈保护机制，防止栈溢出攻击。
- ​**原理**​：在函数栈帧中插入 "canary" 值，如果检测到栈被破坏，程序会终止。
- ​**相关选项**​：
    - `-fstack-protector-strong`（更强的保护）
    - `-fno-stack-protector`（禁用）

---

### ​**3. `-Wall`**​

- ​**作用**​：启用 ​**大多数常见警告**​（但并非全部）。
- ​**包含的警告**​：
    - 未使用的变量 (`-Wunused-variable`)
    - 未使用的函数 (`-Wunused-function`)
    - 可疑的类型转换 (`-Wconversion`)
    - 其他常见代码问题。

---

### ​**4. `-Wunused-but-set-parameter`**​

- ​**作用**​：警告 ​**函数参数被赋值但未使用**。
- ​**示例**​：
    
    cpp
    
    复制
    
    ```cpp
    void foo(int x) { x = 42; }  // x 被赋值但未使用，触发警告
    ```
    

---

### ​**5. `-Wno-free-nonheap-object`**​

- ​**作用**​：​**禁用**​ "尝试释放非堆内存" 的警告。
- ​**背景**​：如果用 `free()` 释放非 `malloc` 分配的内存，GCC 默认会警告。此选项关闭该警告。

---

### ​**6. `-fno-omit-frame-pointer`**​

- ​**作用**​：​**禁止优化掉帧指针（frame pointer）​**。
- ​**用途**​：
    - 调试时保留完整的调用栈。
    - 性能分析工具（如 `perf`）依赖帧指针。

---

### ​**7. `-std=c++0x`**​

- ​**作用**​：启用 ​**C++11 标准**​（旧版 GCC 用 `c++0x`，新版用 `c++11`）。
- ​**现代替代**​：建议改用 `-std=c++17` 或 `-std=c++20`。

---

### ​**8. `-MD` 和 `-MF`**​

- ​**作用**​：生成依赖关系文件（`.d` 文件），用于 Makefile 自动判断是否需要重新编译。
    - `-MD`：生成 `.d` 文件。
    - `-MF`：指定 `.d` 文件的输出路径。

---

### ​**9. 其他常见参数（未列出但可能存在的）​**​

| 参数        | 作用                    |
| --------- | --------------------- |
| `-O2`     | 优化级别 2（平衡性能和编译时间）     |
| `-g`      | 生成调试信息（如 `-g3` 更详细）   |
| `-I<dir>` | 添加头文件搜索路径             |
| `-L<dir>` | 添加库文件搜索路径             |
| `-l<lib>` | 链接指定的库（如 `-lpthread`） |