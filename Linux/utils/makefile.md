
项目分解的意义
- 缩短编译时间
- 利于代码复用
- 利于团队工作

makefile的用途
- 避免复杂命令行
- 减少编译所需时间
- 让编译自动运行

make是解释makefile文件中指令的命令工具。make只对上次编译之后又修改过的文件重新进行编译

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231224215535.png)

make命令按照以下顺序在当前目录搜索makefile文件
- GNUmakefile
- makefile
- Makefile

一般来说，makefile只有一个最终目标：第一条规则中的目标，其它目标服务于此最终目标。


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020231202103207.png)
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231224215428.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231224215414.png)

`.PHONY表示clean是一个伪目标`



![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020231202103358.png)



