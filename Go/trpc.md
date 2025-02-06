
框架采用了基于接口编程的思想，框架只提供了标准接口，由插件来实现具体功能





# BASE

## 定义Naming Service

Naming Service 负责网络通信和协议解析，一个naming service 在寻址上最终用来代表一个\[ip,port,protocol\]的组合，naming service是通过框架配置文件中的server部分的service配置来定义




# Server


## 定义服务接口

采用protobuf来描述一个服务，定义服务方法，请求参数和响应参数

```proto
syntax = "proto3";

package trpc.test.helloworld;

option go_package = "git.woa.com/trpcprotocol/test/helloworld";

service Greeter{
  rpc SayHello (HelloRequest) returns (HelloReply){}
  rpc GetSpecifyMsg(Uid) returns (Item){}
}
message HelloRequest{
  string msg = 1;
}
message  HelloReply{
  string msg = 1;
}

//要查询的Uid
message Uid{
  int32 id = 1;
}


//根据Uid返回的文章信息
message Item{
  int32 id =1; //文章id
  string author = 2; //文章作者
  string content = 3;
  int32 pubtime = 4;
  string title = 5;
}

```

定义了一个Greeter服务，这个服务里面有个SayHello方法，接收一个包含msg字符串的HelloRequest参数，返回HelloReply数据

- syntax 必须是proto3，trpc时基于proto3通信的
- package的内容格式推荐为trpc.{app}.{server}，以trpc为固定前缀，标识这是一个trpc服务协议，app为你的应用名，server为你的服务进程名。
- package后面必须有`option go_package="git.woa.com/trpcprotocol/{app}/{server};`指明你的pb.go文件生成文件的git存放地址，**协议与服务分离**，方便其他人直接引用，git地址用户可以自己随便定
- 定义rpc方法时，一个server（服务进程）可以有多个service（对rpc逻辑分组）


## 生成服务代码

```bash
trpc create -p {name}.proto 
```


```go
// 以下代码在 main.go 文件中，注释为后加的
package main

import (
	_ "git.code.oa.com/trpc-go/trpc-filter/debuglog"
	_ "git.code.oa.com/trpc-go/trpc-filter/recovery"
	trpc "git.code.oa.com/trpc-go/trpc-go"
	"git.code.oa.com/trpc-go/trpc-go/log"
	pb "git.woa.com/trpcprotocol/test/helloworld"
)

func main() {
	// 创建一个服务对象，底层会自动读取服务配置及初始化插件，必须放在 main 函数首行，业务初始化逻辑必须放在 NewServer 后面
	s := trpc.NewServer()
	// 注册当前实现到服务对象中
	pb.RegisterGreeterService(s.Service("trpc.test.helloworld.Greeter"), &greeterImpl{})
	// 启动服务，并阻塞在这里
	if err := s.Serve(); err != nil {
		log.Fatal(err)
	}
}
```

```go
// 以下代码在 greeter.go 文件中，注释为后加的
package main

import (
	"context"

	pb "git.woa.com/trpcprotocol/test/helloworld"
)

type greeterImpl struct {
	pb.UnimplementedGreeter
}

// SayHello 函数入口，用户逻辑写在该函数内部即可
// error 代表的是 exception，异常情况比如数据库连接错误，调用下游服务错误的时候，如果返回 error，rsp 的内容将不再被返回
// 如果业务遇到需要返回的错误码，错误信息，而且同时需要保留 HelloReply，请设计在 HelloReply 里面，并将 error 返回 nil
func (s *greeterImpl) SayHello(ctx context.Context, req *pb.HelloRequest) (*pb.HelloReply, error) {
	rsp := &pb.HelloReply{}
	return rsp, nil
}
```


服务器有一个greeterImpl结构，他通过实现SayHello方法，实现了protobuf服务


以上pb文件生成的桩代码一般通过rick平台管理

## 修改框架配置

```yaml
global:                              # 全局配置
  namespace: Development             # 环境类型，分正式 Production 和非正式 Development 两种类型
server:                              # 服务端配置
  app: test                          # 业务的应用名
  server: helloworld                 # 进程服务名
  service:                           # 业务服务提供的 service，可以有多个
    - name: trpc.test.helloworld.Greeter           # service 的路由名称
      ip: 127.0.0.1                  # 服务监听 ip 地址 可使用占位符 ${ip}，ip 和 nic 二选一，优先 ip
      port: 8000                     # 服务监听端口 可使用占位符 ${port}
      network: tcp                   # 网络监听类型 tcp udp
      protocol: trpc                 # 应用层协议 trpc http
      transport: tnet                 # 要求框架版本 >= 0.11.0，为 tcp trpc 启用 tnet，其他协议可以自行验证
      timeout: 1000                  # 请求最长处理时间 单位 毫秒
```


## 本地启动服务

```shell
go build

./{name} &
```

## 自测联调工具

```sh
trpc-cli -func /trpc.test.helloworld.Greeter/SayHello -target ip://127.0.0.1:8000 -body '{"msg":"hello"}'

trpc-cli -func /trpc.test.getnews.Utility/QueryById -target ip://127.0.0.1:8001 -body '{"id":"1"}'
```

# Client


```go
package main 

import (
	"context"
	
	"git.code.oa.com/trpc-go/trpc-go/client"
	"git.code.oa.com/trpc-go/trpc-go/log"

	pb "git.code.oa.com/trpcprotocol/test/helloworld" 
	// 被调服务的协议生成文件 pb.go 的 git 地址，没有 push 到 git 的话，可以在 gomod 里面 replace 本地路径进行引用，如 gomod 里面加上一行：replace "git.code.oa.com/trpcprotocol/test/helloworld" => ./你的本地桩代码路径
)

func main() {
	proxy := pb.NewGreeterClientProxy() // 创建一个客户端调用代理，名词解释见客户端开发文档
	req :=  &pb.HelloRequest{Msg: "Hello, I am tRPC-Go client."} // 填充请求参数
	rsp, err := proxy.SayHello(context.Background(), req, client.WithTarget("ip://127.0.0.1:8000")) // 调用目标地址为前面启动的服务监听的地址
	if err != nil {
		log.Errorf("could not greet: %v", err)
		return
	}
	log.Debugf("response: %v", rsp)
}
```


# 使用gorm

使用gorm去连接数据库时，调用方作为client，数据库端作为server，因此配置项要加在client端内

```yaml
client:                                            
  service: 
    - name: trpc.mysql.server.service
      # 写法参考：https://github.com/go-sql-driver/mysql?tab=readme-ov-file#dsn-data-source-name
      target: dsn://root:123456@tcp(127.0.0.1:3306)/mydb?charset=utf8mb4&parseTime=True
```

分模块写法，在dao.go 文件中，先去初始化一个db连接
```go
import (
	"fmt"
	tGorm "git.code.oa.com/trpc-go/trpc-database/gorm"//trpc-go封装的gorm
	. "git.woa.com/qingruixu/server/entity"
	_ "github.com/go-sql-driver/mysql"
	"gorm.io/gorm" //原生的gorm，接收返回的*gorm.DB
)



var db *gorm.DB

func InitDB() {
    	var err error
    	db, err = tGorm.NewClientProxy("trpc.mysql.server.service")
    	if err != nil {
    		panic(err)
    	}
}
```

main.go中

***业务初始化逻辑必须放在 NewServer 后面***

```go
func main() {
    	s := trpc.NewServer()
    	dao.InitDB()//数据库初始化
	}
```

