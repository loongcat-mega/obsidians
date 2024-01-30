 wc -cl filename  统计filename的行数和字符数
 head filename 
 tail filename  查看文件头/尾 默认10行内容
 head -n num filename  查看文件前num行内容
 less filename  智能显示文件内容
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231228193939.png)


## elf格式

`7F 45 4C 46`

ELF 就是 **Executable and Linkable Format**，它定义了可执行文件、库文件和 core 文件的结构。这种格式能让操作系统正确解释文件中的机器指令。 ELF 文件是编译器或链接器输出的二进制格式。使用合适的工具，可以更好的分析和理解这种文件


## file  辨识文件类型

查看可执行文件是ARM架构还是X86架构
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231228192540.png)

## strings  打印可输出字符

**strings命令** 在对象文件或二进制文件中查找可打印的字符串。字符串是4个或更多可打印字符的任意序列，以换行符或空字符结束。 strings命令对识别随机对象文件很有用

```bash
Usage: strings [option(s)] [file(s)]
 Display printable strings in [file(s)] (stdin by default)
 The options are:
  -a - --all                Scan the entire file, not just the data section [default]
  -d --data                 Only scan the data sections in the file
  -f --print-file-name      Print the name of the file before each string
```

可查看文件中所有的符号，包括编译器版本信息
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020231203205644.png)

在大型的软件开发中， 假设有100个.c/.cpp文件， 这个.cpp文件最终生成10个.so库， 那么怎样才能快速知道某个.c/.cpp文件编译到那个.so库中去了呢？ 当然， 你可能要说， 看makefile不就知道了。 对， 看makefile肯定可以， 但如下方法更好， 直接用命令：

`strings -f "*.so" | grep "xxxxxx"`


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020231203205847.png)
## nm  列出符号表

nm命令是linux下针对某些特定文件的分析工具，能够列出**库文件**（_.a、_.lib）、**目标文件**（*.o）、**可执行文件**的符号表
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020231203205941.png)
符号类型大写代表全局符号，小写代表本地符号
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020231203210402.png)
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020231203210413.png)

## objdump  显示二进制文件信息

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020231203210813.png)


### 显示 `mytest.o` 文件中的 `text` 段的内容:
```text
objdump --section=.text -s mytest.o
```

### 反汇编 `mytest.o` 中的 `text` 段内容，并尽可能用源代码形式表示
```text
objdump -j .text -S mytest.o
```

### 显示文件的符号表入口

```text
objdump -t mytest.o
```

## readelf

## xxd  查看文件内容的十六进制表示

默认转16进制
```bash
-b 转二进制
-r 十六进制转二进制
```