
```go
var _ Pooler = (*ConnPool)(nil)
```
用于检测`*ConnPool`是否实现了`Pooler`接口的所有方法

```go
select {  
case <-ctx.Done():  
    return ctx.Err()  
default:  
}
```
检测ctx是否超时

```go
cn = p.idleConns[0]  
copy(p.idleConns, p.idleConns[1:])  
p.idleConns = p.idleConns[:n-1]
```
使用copy防止`idleConns`被引用导致数据泄露问题
因为有`p`；里有len的限制，所以前len个元素保证有效


```go
queue chan struct{}
func (p *ConnPool) waitTurn(ctx context.Context) error {  
    select {  
    case <-ctx.Done():  
       return ctx.Err()  
    default:  
    }  
  
    select {  
    case p.queue <- struct{}{}:  
       return nil  
    default:  
    }  
  
    timer := timers.Get().(*time.Timer)  
    timer.Reset(p.cfg.PoolTimeout)  
  
    select {  
    case <-ctx.Done():  
       if !timer.Stop() {  
          <-timer.C  
       }  
       timers.Put(timer)  
       return ctx.Err()  
    case p.queue <- struct{}{}:  
       if !timer.Stop() {  
          <-timer.C  
       }  
       timers.Put(timer)  
       return nil  
    case <-timer.C:  
       timers.Put(timer)  
       atomic.AddUint32(&p.stats.Timeouts, 1)  
       return ErrPoolTimeout  
    }  
}  
  
func (p *ConnPool) freeTurn() {  
    <-p.queue  
}
```
多个线程竞争一个资源，可以用令牌的形式


```go
// Len returns total number of connections.
func (p *ConnPool) Len() int {  
    p.connsMu.Lock()  
    n := len(p.conns)  
    p.connsMu.Unlock()  
    return n  
}
```
多线程环境下获取切片长度要加锁


```go
func (p *ConnPool) tryDial() {  
    for {  
       if p.closed() {  
          return  
       }  
  
       conn, err := p.cfg.Dialer(context.Background())  
       if err != nil {  
          p.setLastDialError(err)  
          time.Sleep(time.Second)  
          continue  
       }  
  
       atomic.StoreUint32(&p.dialErrorsNum, 0)  
       _ = conn.Close()  
       return  
    }  
}
```
重试：固定时间间隔的退避策略


```go
// BytesToString converts byte slice to string.
func BytesToString(b []byte) string {  
    return *(*string)(unsafe.Pointer(&b))  
}  
  
// StringToBytes converts string to byte slice.
func StringToBytes(s string) []byte {  
    return *(*[]byte)(unsafe.Pointer(  
       &struct {  
          string  
          Cap int  
       }{s, len(s)},  
    ))  
}
```
string与byte转换


```go
func Sleep(ctx context.Context, dur time.Duration) error {  
    t := time.NewTimer(dur)  
    defer t.Stop()  
  
    select {  
    case <-t.C:  
       return nil  
    case <-ctx.Done():  
       return ctx.Err()  
    }  
}
```
带有ctx的sleep,重试时使用


```go
func (o *Once) Do(f func() error) error {  
    if atomic.LoadUint32(&o.done) == 1 {  
       return nil  
    }  
    // Slow-path.  
    o.m.Lock()  
    defer o.m.Unlock()  
    var err error  
    if o.done == 0 {  
       err = f()  
       if err == nil {  
          atomic.StoreUint32(&o.done, 1)  
       }  
    }  
    return err  
}
```
once.do

使用 Buffer 拼接字符串