
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240115095205.png)

http1.0:为每一个请求建立一个连接，每个连接要经历三次握手与四次挥手，一个连接传递一个文件
http1.1:一个连接传递多个文件
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240115095116.png)


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240115095601.png)
## 请求方式



### POST 增

向指定资源提交数据，通常会导致服务器端的状态发生变化。例如在Web表单中填写用户信息并提交时，就是使用POST请求方式将表单数据提交到服务器存储
使用POST请求方式提交的数据会被包含在请求体中，而不像GET那样包含在URL中。因此POST可以提交比GET更大的数据量，并且相对更安全

```makefile
POST /api/user HTTP/1.1
Host: example.com
Content-Type: application/json
Content-Length: 123

{
   "name": "John Doe",
   "email": "johndoe@example.com",
   "age": 30
}
```
上述代码表示向 `example.com` 的 `/api/user` 资源发送一个 `POST` 请求，请求体中包含了一个 JSON 格式的用户信息

```python
import requests
# 目标网站的URL
url = "http://47.95.4.132:8000/items/42"
data = {"content": "Example Content"}
# 发送POST请求
data["content"]=input()
response = requests.post(url, json=data)
# 打印响应状态码和内容
#print("Response Status Code:", response.status_code)
print("Response Content:", response.json())
```


### DELETE 删

用于请求服务器删除指定的资源

```bash
DELETE /api/user?id=123 HTTP/1.1
Host: example.com
```

上述代码表示向 `example.com` 的 `/api/user` 资源发送一个 `DELETE` 请求，并指定要删除的用户的 ID



### PUT 改

向服务器更新指定资源，可以理解为对服务器上的资源进行修改操作，使用PUT请求会覆盖原有的资源内容

```makefile
PUT /api/user HTTP/1.1
Host: example.com
Content-Type: application/json
Content-Length: 123

{
   "id": 123,
   "name": "John Doe",
   "email": "johndoe@example.com",
   "age": 30
}
```

上述代码表示向 `example.com` 的 `/api/user` 资源发送一个 `PUT` 请求，并指定要更新的用户信息。

```python
import requests
# 目标网站的URL
url = "http://47.95.4.132:8000/items/42"
data = {"content": "Example Content"}
# 发送POST请求
data["content"]=input()
response = requests.put(url, json=data)
# 打印响应状态码和内容
#print("Response Status Code:", response.status_code)
print("Response Content:", response.json())
```

### GET 查

用于向指定资源发出请求，请求中包含了资源的URL和请求参数。服务器通过解析请求参数来返回相应的资源，不会改变服务器端的状态。
使用GET请求方式提交的数据会被包含在URL中，因此易于被缓存和浏览器保存，但也因此不适合用于提交敏感数据

```bash
GET /api/user?id=123 HTTP/1.1
Host: example.com
```
上述代码表示向 `example.com` 的 `/api/user` 资源发送一个 `GET` 请求，请求参数中包含了用户的 ID。

```python
import requests
# 目标网站的URL
url = "http://47.95.4.132:8000/items/42"
data = {"content": "Example Content"}
# 发送POST请求
data["content"]=input()
response = requests.get(url)
# 打印响应状态码和内容
#print("Response Status Code:", response.status_code)
print("Response Content:", response.json())
```