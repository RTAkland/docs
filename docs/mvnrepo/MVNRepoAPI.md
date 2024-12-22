# MVNRepoAPI

> 这里是MVNRepo的API文档

# 前言

所有的API端点都是通过JWT的方式认证的, 登录见[登录](#登录)

# 登录

POST到`/-/api/login`请求体携带以下格式的内容

```json
{
  "username": "admin",
  "password": "admin"
}
```

服务端会返回下面格式的json数据

```json
{
  "access_token": "xxxxx",
  "expires_in": 7200
}
```

然后访问任意一个端点并且带上`Authorization`头部, 值为`Bearer xxxxx`即可

# 用户

## 添加用户

> 对用户的所有操作都需要使用默认的admin账号来操作

POST到`/-/api/user`请求体携带以下格式的内容

```json
{
  "username": "xxxxxx",
  "password": "111111"
}
```

> 这样会创建一个用户

## 修改用户信息

携带同样的json数据到同一个端点但是方法是`PUT`
即可修改密码或者账号

## 删除用户

只需要携带以下数据到同一个端点并且使用`DELETE`方法

```json
{
  "username": "xxxxxx"
}
```

# 仓库

## 删除包

使用`DELETE`方法发送到`/-/api/artifacts`并且携带以下格式的json数据

```json
{
  "artifactId": "example-artifact",
  "groupId": "com.example",
  "version": "1.0.0",
  "repository": "releases"
}
```

## 浏览包目录

用`GET`方法请求同一个端点, 服务器会返回下面格式的数据

```json
{
  "data": [
    {
      "name": "releases",
      "idDirectory": true,
      "size": 114514
    },
    {
      "name": "snapshots",
      "idDirectory": true,
      "size": 1919810
    }
  ]
}
```

这两个代表的是仓库名, size是文件的大小单位是Byte字节, 如果是文件夹的话则为0
继续在URI中添加仓库名, 就像这样`http://127.0.0.1:8088/-/api/artifacts/releases`
服务端还是会返回所有的文件列表格式和上方一样

直到访问的是文件会直接下载

> 在URL加上`?limit=10`参数可以限制查询的最大数量, 不添加默认为100, 无上限

## 包下载量统计

`GET`请求发送到`/-/api/statistics`并且加上查询参数`artifactId`, `groupId`, `repository`, 就像下面这样

```shell
$ curl http://127.0.0.1:8088/-/api/statistics?group=cn.rtast&repository=releases&artifactId=test-publish-4
```

服务端会返回以下格式的json数据

```json
{
  "group": "cn/rtast",
  "artifactId": "test-publish-4",
  "repository": "releases",
  "downloadCount": 2
}
```

## 搜索包

> 此API端点需要认证

`GET`请求发送到`/-/api/artifacts/search`并添加查询参数`artifactId`就像下面这样

```shell
$ curl http://127.0.0.1:8088/-/api/artifacts/search?keyword=test-publish&type=<类型>
```

> 这里的类型是字符串枚举, 可以是`artifactId`, `groupId`, `version`, `repository`, `author`


服务器会返回下面格式的json数据

```json
{
  "message": "搜索到1个结果",
  "code": 200,
  "count": 1,
  "data": [
    {
      "groupId": "cn/rtast",
      "artifactId": "test-publish-3",
      "repository": "releases",
      "createdBy": "admin",
      "lastUpdated": 1734691431,
      "version": "0.0.1"
    }
  ]
}
```

# 程序信息

> 所有端点都需要认证

## 所有包占用的存储空间

`GET`方法发送到`/-/api/storage`服务端会返回下面格式的json数据

```json
{
  "usedSize": 167275
}
```

> 单位是字节(byte)

## 程序运行信息

`GET`方法发送到`/-/api/status`服务端会返回下面格式的json数据

```json
{
  "jvmVersion": "17.0.13+11-LTS",
  "totalMemory": 41943040,
  "freeMemory": 12844984,
  "maxMemory": 4118806528,
  "kotlinVersion": "2.1.0",
  "osName": "Windows 11",
  "osArch": "amd64",
  "osVersion": "10.0"
}
```