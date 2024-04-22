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

