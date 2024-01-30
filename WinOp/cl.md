## 环境变量配置

-    `Path`环境变量
    

添加`cl.exe`的路径到`Path`环境变量的目的是使命令行能找到`cl.exe`。在`Path`环境变量中添加如下条目：

```
C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\bin\Hostx64\x64
```

-   `INCLUDE`环境变量
    

添加`Path`环境变量的目的是使编译器找到`include`文件夹。在系统变量中新建`INCLUDE`环境变量（注意大写）。  
添加如下条目：

```
C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\include
C:\Program Files (x86)\Windows Kits\10\Include\10.0.17763.0\shared
C:\Program Files (x86)\Windows Kits\10\Include\10.0.17763.0\ucrt
C:\Program Files (x86)\Windows Kits\10\Include\10.0.17763.0\um
C:\Program Files (x86)\Windows Kits\10\Include\10.0.17763.0\winrt

C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\include;C:\Program Files (x86)\Windows Kits\10\Include\10.0.17763.0\shared;C:\Program Files (x86)\Windows Kits\10\Include\10.0.17763.0\ucrt;C:\Program Files (x86)\Windows Kits\10\Include\10.0.17763.0\um;C:\Program Files (x86)\Windows Kits\10\Include\10.0.17763.0\winrt
```

-    `LIB`环境变量
    

在系统变量中新建`LIB`环境变量（注意大写）。  
添加如下条目：

```
C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\lib\x64
C:\Program Files (x86)\Windows Kits\10\Lib\10.0.17763.0\ucrt\x64
C:\Program Files (x86)\Windows Kits\10\Lib\10.0.17763.0\um\x64


C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\lib\x64;C:\Program Files (x86)\Windows Kits\10\Lib\10.0.17763.0\ucrt\x64;C:\Program Files (x86)\Windows Kits\10\Lib\10.0.17763.0\um\x64

```


## Usage

```bash
/c: 只进行编译而不进行链接 compile-only
当使用 /c 选项时，编译器将执行以下步骤：
预处理：处理源代码中的预处理指令，例如 #include 和 #define。
编译：将预处理后的源代码编译为汇编代码（.asm 文件）。
汇编：将汇编代码转换为目标文件（.obj 文件）

/Fo: 指定对象文件的名称。Fo 表示 "File, Object"。
cl /FoMyObjectFile.obj MySourceFile.c

/Fa: 指定汇编输出的文件名。Fa 表示 "File, Assembly"。
cl /FaMyAssembly.asm MySourceFile.c

/Fe: 指定可执行文件的名称。Fe 表示 "File, Executable"。
cl /FeMyExecutable.exe MySourceFile.c

/Fi: 指定预处理的文件的名称。Fi 表示 "File, Include"。
cl /FiMyPreprocessedFile.i MySourceFile.c

```



### 预处理

```bash
cl /E /P /C /Tc<输入的C文件名> /Fi<输出的预处理文件名>

```
-   `/E` 选项用于指示编译器只进行预处理。
-   `/P` 选项指示编译器生成预处理后的代码到标准输出。
-   `/C` 选项指示编译器不进行编译、汇编或链接。
-   `/Tc` 选项用于指定输入的 C 文件名。
-   `/Fi` 选项用于指定输出的预处理文件名。





### 编译
```bash
cl /c /Fa<输出的汇编文件名> <输入的C文件名>
cl /c /Od /Fanassembly.asm example.c

```
-   `/c` 选项指示编译器只进行编译而不进行链接，生成目标文件。
-   `/Fa` 选项用于指定输出的汇编文件的名称
-   `/Od` 选项用于指示编译器不进行优化。


### 汇编

