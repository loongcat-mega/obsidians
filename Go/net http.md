
Go实现向服务器发送http请求，服务器响应并返回指定请求内容
- go实现http服务器端处理请求`net/http`
- go与mysql的数据请求
- 返回json格式，如何打包封装
- go mod管理go文件

gorm

边界条件等的判断


context原理

rpc介绍
proto3知识




# Server

ListenAndServer使用指定的监听地址和处理器启动一个HTTP服务器，


```go
package main

import (
	"fmt"
	"net/http"
	"strconv"
	//"io/ioutil"
)

func SayHello(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "hello world")
}

func SayRoot(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "this is root")
}
func GetHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Println("method:", r.Method)
	q := r.URL.Query()

	if len(q) != 0 {

		id, err := strconv.Atoi(q.Get("id"))
		if err != nil {
			panic(err)
		}
		fmt.Fprintln(w, reps[id])

	}
	//content =

}

type News struct {
	title, content, author string
	pubtime                int
}

var reps = make(map[int]News, 100)

func init() {
	reps[0] = News{title: "a", author: "author1", content: "news", pubtime: 20}
	reps[1] = News{title: "b", author: "author2", content: "news2", pubtime: 30}
}

func main() {

	fmt.Println("Server already open")
	http.HandleFunc("/get", GetHandler)
	http.HandleFunc("/", SayRoot)
	//http.HandleFunc("/", SayRoot)

	err := http.ListenAndServe(":9999", nil)
	if err != nil {
		fmt.Println("Error")
		return
	}
}

```