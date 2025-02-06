
go modules 是golang 1.11引入的新特性。模块是相关Go包的集合。modules是源代码交换和版本控制的单元，go命令直接支持使用modules，包括记录和解析对其他模块的依赖性，替换旧的基于GOPATH的方法来指定在给定构建中使用哪些源文件

GO111MODULE有三个值，off on 和 auto。在使用模块的时候，GOPATH是无意义的，不过它还是会把下载的依赖储存在$GOPAH/src/mod 中，也会把go install 的结果放在 $GOPATH/bin 中

- off ：无模块支持，go会从GOPATH和vendor文件夹寻包
- on ：模块支持，go 会忽略GOPATH和vendor文件夹，只根据go.mod下载依赖
- auto ： 在$GOPAH/src 外面且根目录有go.mod文件时，开启模块支持



## go mod

摒弃vendor和GOPATH，拥抱本地库。

go mod 命令
- download : 下载依赖的module到本地Cache,$GOPATH/pkg/mod
- edit
- graph：打印模块依赖图
- init ：初始化一个新的module，创建go.mod文件
- tidy
- vendor：将依赖复制到vendor下
- why 解释为什么需要依赖



go.mod 提供了四个命令
- module：语句指定包的名字（路径）
- require：指定的依赖项模块
- replace：替换依赖项模块
- exclude：忽略依赖项模块


go module 安装package的原则是先拉最新的release tag，若无tag则拉最新的commit，go会自动生成一个go.sum文件来记录dependdency tree

