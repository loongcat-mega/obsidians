

字符串读取规则：解析第一个字符，检查是否是字符串
- 如果是，则读取`[1:]`以`\n`分割的首个子串，并返回`[:-2]`。解析返回的长度。然后读取字符串,并返回
- 如果不是，直接返回
## readLine
```go
func (r *Reader) readLine() ([]byte, error)

b, err := r.rd.ReadSlice('\n')
return b[:len(b)-2], nil
```
## readStringReply

![QQ_1739502738891.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/QQ_1739502738891.png)
```go
func (r *Reader) readStringReply(line []byte) (string, error) {  
    n, err := replyLen(line)  
    if err != nil {  
       return "", err  
    }  
  
    b := make([]byte, n+2)  
    _, err = io.ReadFull(r.rd, b)  
    if err != nil {  
       return "", err  
    }  
  
    return util.BytesToString(b[:n]), nil  
}
```
## readSlice

![QQ_1739503525594.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/QQ_1739503525594.png)

```go
func (r *Reader) readSlice(line []byte) ([]interface{}, error) {  
    n, err := replyLen(line)  
    if err != nil {  
       return nil, err  
    }  
  
    val := make([]interface{}, n)  
    for i := 0; i < len(val); i++ {  
       v, err := r.ReadReply()  
       if err != nil {  
          if err == Nil {  
             val[i] = nil  
             continue          }  
          if err, ok := err.(RedisError); ok {  
             val[i] = err  
             continue  
          }  
          return nil, err  
       }  
       val[i] = v  
    }  
    return val, nil  
}
```