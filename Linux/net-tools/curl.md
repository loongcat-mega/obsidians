```shell
curl  url
默认是GET请求

curl -XPOST url 
发送POST请求

curl -XPOST url -d 数据（'{type:"json"}'）

 curl -XPOST 127.0.0.1:8000/v1/article/operate/delete -d '{"article_id":"aaa","operation":"delete"}'

curl -XPOST -H "Content-Type: application/json" http://127.0.0.1:8000/v1/unideletion/article/operate -d '{"article_id":"aaa","operation":"delete","type":"article","auth":{"app_id":"3"}}'

curl -XPOST -H "Content-Type: application/json" http://127.0.0.1:8000/v1/unideletion/article/operate -d '{"article_id":"aaa","operation":"1"}'

curl -XPOST -H "Content-Type: application/json" http://127.0.0.1:8000/v1/unideletion/article/getstatus -d '{"article_id":"ccc"}'

curl -XPOST -H "Content-Type: application/json" http://127.0.0.1:8000/v1/unideletion/article/getoptrecords -d '{"article_id":"ccc"}'

curl -XPOST -H "Content-Type: application/json" http://127.0.0.1:8000/v1/article/operate/delete -d '{"article_id":"aaa","operation":"delete"}'


curl -XPOST -H "Content-Type: application/json" http://api.webdev.com/v1/entity/get  -d '{
    "appid": "10000",
    "entity_id": "20240617A06CJ700",
    "entity_type": "article",
    "nonce": "bQBTmW",
    "signature": "df9d78c109c4ded1cc36d252edc387a7e0a5babc",
    "timestamp": "1718768513",
    "focus_keys": "abstract,ai_tag_item_list"
}'

curl -XPOST   -H  "Content-Type: application/json" http://api.webdev.com/v1/entity/get -d '{"appid": "10000","entity_id": "20240617A06CJ700","entity_type": "article","nonce": "bQBTmW","signature": "df9d78c109c4ded1cc36d252edc387a7e0a5babc","timestamp": 1718768513,"focus_keys": "abstract,ai_tag_item_list"}'

/v1/article/operate/getrecord

curl -XPUT url -d 数据（'{type:"json"}'）
更新数据

获取报文首部
curl -I URL

将内容（png等）下载到当前文件夹，并自定义文件名为alias
curl -o alias url

恢复中断的下载
curl -C - url

跟随重定向
curl url -L

```