
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20250522133016.png)

# Reader

```go
// Reader implements buffering for an io.Reader object.type Reader struct {  
    buf          []byte  
    rd           io.Reader // reader provided by the client  
    r, w         int       // buf read and write positions    err          error  
    lastByte     int // last byte read for UnreadByte; -1 means invalid  
    lastRuneSize int // size of last rune read for UnreadRune; -1 means invalid  
}
```


## Size
```go
func (b *Reader) Size() int { return len(b.buf) }
```
返回缓冲区大小

## Buffered
```go
func (b *Reader) Buffered() int { return b.w - b.r }
```
当前缓冲区可以读取（未读取）的数据
## Reset
```go
func (b *Reader) Reset(r io.Reader)
```
将rd切换到r，并重置所有状态

1. 比较b与r是否是同一个
2. 切换* b
## Read
```go
func (b *Reader) Read(p []byte) (n int, err error)
```
读取数据到切片p中，返回读取的字节数和错误
1. 如果p长度为0，返回0
2. 如果buf中没有待读取的数据，即r == w，则看p的大小与buf大小，如果p大buf小，则直接读取数据到p中，减少数据拷贝。如果p小buf大，则读取数据到buf中
3. 将buf中的数据拷贝到p中，返回`n = copy(p, b.buf[b.r:b.w])`

## ReadSlice
```go
func (b *Reader) ReadSlice(delim byte) (line []byte, err error)
```
返回缓冲区中，缓冲区开头到delim的数据切片，包含delim
```go
s := 0 // search start index  
for {  
    // Search buffer.  
    if i := bytes.IndexByte(b.buf[b.r+s:b.w], delim); i >= 0 {  
       i += s  
       line = b.buf[b.r : b.r+i+1]  
       b.r += i + 1  
       break  
    }
    ...
}
```
如果缓冲区满了仍然没有找到delim，则返回ErrBufferFull

## ReadBytes
```go
func (b *Reader) ReadBytes(delim byte) ([]byte, error)
```
找到rd中以delim分割的数据切片，如果缓冲区满了，则暂存当前缓冲区，并继续读取数据至缓冲区，直至找到delim

## ReadString
```go
func (b *Reader) ReadString(delim byte) (string, error) {  
    full, frag, n, err := b.collectFragments(delim)  
    // Allocate new buffer to hold the full pieces and the fragment.  
    var buf strings.Builder  
    buf.Grow(n)  
    // Copy full pieces and fragment in.  
    for _, fb := range full {  
       buf.Write(fb)  
    }  
    buf.Write(frag)  
    return buf.String(), err  
}
```
返回ReadBytes的字符串形式
## ReadByte
```go
func (b *Reader) ReadByte() (byte, error)
```
读取一个字节
```go
c := b.buf[b.r]  
b.r++  
b.lastByte = int(c)
```
## UnreadByte
```go
func (b *Reader) UnreadByte() error
```
撤回最后读取的一个字节
```go
b.buf[b.r] = byte(b.lastByte)  
b.lastByte = -1  
```

## fill
```go
func (b *Reader) fill()
```
读取数据直至填满缓冲区

1. 将buf未读取的数据移至开头
2. 持续重试直到读取出有效数据到`n, err := b.rd.Read(b.buf[b.w:])`

