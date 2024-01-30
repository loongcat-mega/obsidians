fastapi是一个现代、快速(高性能)的Web框架，用于基于标准python类型提示使用pyhton3.8+构建api

主要特点：
- 快速 高性能
- 快速编码
- 更少的错误
- 直观 简单

## Conception

「路径」也通常被称为「端点」或「路由」。
开发 API 时，「路径」是用来分离「关注点」和「资源」的主要手段

### 创建FastAPI实例

```python
app = FastAPI()
```

### 定义一个路径操作装饰器

`@app.get("/")` 告诉 **FastAPI** 在它下方的函数负责处理如下访问请求：

-   请求路径为 `/`
-   使用 `get` 操作

### 定义路径操作函数

```python
@app.get("/")
#路径操作函数
def root():
    return {"message": "Hello World"}
```


## 安装

```bash
pip install fastapi
pip install "uvicorn[standard]"
```

## Usage
```python
from typing import Union

from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"Hello": "World"}
	

@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}
```
run it

```bash
uvicorn main:app --host 0.0.0.0 --port 8000 --reload

$ uvicorn main:app --reload

INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
INFO:     Started reloader process [28720]
INFO:     Started server process [28722]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
```

访问URL：http://127.0.0.1:8000/docs，你会看到自动生成的交互式 API 文档，由Swagger UI 生成

### 上传文件

```python
from fastapi import FastAPI, File, UploadFile

app = FastAPI()


@app.post("/files/")
async def create_file(file: bytes = File()):
    return {"file_size": len(file)}


@app.post("/uploadfile/")
async def create_upload_file(file: UploadFile):
    return {"filename": file.filename}

```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240119153857.png)

### 上传文件并保存到服务器

```python
from fastapi import FastAPI, File, UploadFile
from pathlib import Path
app = FastAPI()

# 设置上传文件保存目录
UPLOAD_DIR = Path(".")
UPLOAD_DIR.mkdir(exist_ok=True)
@app.post("/files/")
async def create_file(file: bytes = File()):
    return {"file_size": len(file)}


@app.post("/uploadfile/")
async def create_upload_file(file: UploadFile):
    filename = file.filename

    # 构建保存路径
    save_path = UPLOAD_DIR / filename

    # 保存文件
    with save_path.open("wb") as buffer:
        buffer.write(file.file.read())
    return {"filename": file.filename}

```
