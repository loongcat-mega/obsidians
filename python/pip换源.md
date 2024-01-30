### 国内比较好的PyPI源库有：

```python
清华大学：https://pypi.tuna.tsinghua.edu.cn/simple
阿里云：http://mirrors.aliyun.com/pypi/simple
豆瓣：http://pypi.douban.com/simple
```


## 临时使用

```bash
 pip install -i https://pypi.tuna.tsinghua.edu.cn/simple manimgl
```

## 永久使用

### windows

比如windows账号是 admin，那么建立 admin主目录下的 pip子目录，在此pip子目录下建立pip的配置文件：pip.ini

`c:\users\admin\pip\pip.ini`

```ini
#阿里云：http://mirrors.aliyun.com/pypi/simple/
[global]
index-url = https://repo.huaweicloud.com/repository/pypi/simple
trusted-host = repo.huaweicloud.com
timeout = 120

#清华大学：https://pypi.tuna.tsinghua.edu.cn/simple
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host = pypi.tuna.tsinghua.edu.cn
#豆瓣：http://pypi.douban.com/simple/

# 换回默认源  
#pip config unset global.index-url
```
### Linux 

Linux环境下：同样比例账号为 admin 则需要建立子目录 `\home\admin\pip`; 并在此pip目录下建立内容同上的 pip.conf的位置文件。

```bash
cd ~
mkdir pip
cd pip
vi pip.ini
#内容同windows环境下。
```