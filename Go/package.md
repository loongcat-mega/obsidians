
包简单理解成包含了多个	.go文件的目录

每段go程序都必须属于一个包，一个标准的可执行的Go程序必须有package main 的声明。如果一段程序属于main包的，那么执行go install 的时候就会将其生成二进制文件，当执行这个文件的时候，就会调用main函数。当一段程序是属于main以外的包，执行go install的时候就会创建一个包管理文件。

位于所有代码第一行的 **包的声明**  并非一定要与包同名。当引用包的时候，需要使用包的声明来作为引用变量


包的命名规范：避免使用下划线，中划线或者掺杂大写字母

创建包：目录下的文件包名声明为目录名

```shell
qingruixu@QINGRUIXU-MB0 config % pwd
/Users/qingruixu/GOMODUE/config
qingruixu@QINGRUIXU-MB0 config % cat config.go 
package config

var Disc = "this is config"
qingruixu@QINGRUIXU-MB0 config % 
```

引用包：go mod创建的模块名+目录

```shell
qingruixu@QINGRUIXU-MB0 GOMODUE % cat main.go 
package main

import (
        "fmt"
        "mygomodule/config"
        "mygomodule/entity"
)

func main() {
        fmt.Println(config.Disc)
        fmt.Println(entity.Disc)

}
qingruixu@QINGRUIXU-MB0 GOMODUE % cat go.mod 
module mygomodule

go 1.22.3
qingruixu@QINGRUIXU-MB0 GOMODUE % 
```

scope：指代码块中可以访问已定义变量的区域
包的scope是指在一个包中可以访问已定义变量的区域，这个区域是包中所有文件的顶层块