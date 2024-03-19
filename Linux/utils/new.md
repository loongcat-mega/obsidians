

## 安装conda

### 安装依赖包
```shell
apt install libgl1-mesa-glx libegl1-mesa libxrandr2 libxrandr2 libxss1 libxcursor1 libxcomposite1 libasound2 libxi6 libxtst6
```
### 获取Anaconda并安装
```shell
wget https://repo.anaconda.com/archive/Anaconda3-2022.10-Linux-x86_64.sh

bash Anaconda3-2022.10-Linux-x86_64.sh
```

### 创建虚拟环境

```shell
conda create -n name python=3.10
```

##  cuda

### cuda版本

PyTorch  2.1.0
```
https://download.pytorch.org/whl/torch/
```
Python  3.10(ubuntu22.04)

Cuda  12.1


### cuda是否可用
```python
import torch
print(torch.__version__)
print(torch.cuda.is_available())
```
## ssh

```shell
sudo apt-get install openssh-server
sudo ps -e |grep ssh
sudo service ssh start
sudo service ssh stop
sudo service ssh restart

ssh-keygen
```

## fastapi

```shel
pip install fastapi
pip install "uvicorn[standard]"

run
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

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

### 测试用

```shell
curl http://182.92.170.252:8000/items/$(python -c 'import urllib.parse; print(urllib.parse.quote("你"))')

```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240227143336.png)

## model && fastapi
```python
from textgen import GptModel
from typing import Union

from fastapi import FastAPI

app = FastAPI()

def generate_prompt(instruction):
    return f"""A chat between a curious user and an artificial intelligence assistant. The assistant gives helpful, detailed, and polite answers to the user's questions.USER: {instruction} ASSISTANT: """

path=r"/root/longcat"
model = GptModel("llama", path)


@app.get("/")
def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    predict_sentence = generate_prompt(q)
    r = model.predict([predict_sentence])
    return {"item_id": item_id, "q": r}
print(r) # ["1、首先大多数小儿退热药中含有解热镇痛成分阿司匹林或布洛芬等，这类药品虽然副作用较少..."]


```


## frp

### server

`frps.toml`
```toml
bindPort = 7000
```

#### 启动服务

```bash
./frps -c ./frps.toml
or
# 启动frp
sudo systemctl start frps
# 停止frp
sudo systemctl stop frps
# 重启frp
sudo systemctl restart frps
# 查看frp状态
sudo systemctl status frps
# 设置为开机自启
sudo systemctl enable frps

```


### client


`frpc.toml`
```toml
serverAddr = "47.95.4.132"  
serverPort = 7000  
  
[[proxies]]  
name = "test-tcp"  
type = "tcp"  
localIP = "127.0.0.1"  
localPort = 22  
remotePort = 6000
```
将公网6000端口映射到私网22端口

#### 启动服务

```bash
./frpc -c ./frpc.toml
```
## MedicalGPT

```shell
git clone https://github.com/shibing624/MedicalGPT
cd MedicalGPT
pip install -r requirements.txt --upgrade
```

## vicuna-baichuan-13b-chat
### 安装testgen
```shell
pip install -U textgen
```
### 自动下载模型
```python
from textgen import GptModel

def generate_prompt(instruction):
    return f"""A chat between a curious user and an artificial intelligence assistant. The assistant gives helpful, detailed, and polite answers to the user's questions.USER: {instruction} ASSISTANT: """

model = GptModel("baichuan", "shibing624/vicuna-baichuan-13b-chat")
predict_sentence = generate_prompt("一岁宝宝发烧能吃啥药?")
r = model.predict([predict_sentence])
print(r) # ["1、首先大多数小儿退热药中含有解热镇痛成分阿司匹林或布洛芬等，这类药品虽然副作用较少..."]

```

## ziya-llama-13b-medical-merged

```python
from textgen import GptModel

def generate_prompt(instruction):
    return f"""Below is an instruction that describes a task. Write a response that appropriately completes the request.\n\n### Instruction:{instruction}\n\n### Response: """


model = GptModel("llama", "shibing624/ziya-llama-13b-medical-merged")
predict_sentence = generate_prompt("一岁宝宝发烧能吃啥药?")
r = model.predict([predict_sentence])
print(r) # ["1、首先大多数小儿退热药中含有解热镇痛成分阿司匹林或布洛芬等，这类药品虽然副作用较少..."]

```
## apaca7B

```txt
autuDL:
frpc
fastapi

aliyun:
frps

```

