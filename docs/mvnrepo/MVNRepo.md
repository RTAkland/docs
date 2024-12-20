# MVNRepo

> 这里是MVNRepo的使用文档

# 构建jar

```shell
$ chmod +x ./gradlew  // Windows上可选
$ ./gradlew build
$ cd mvnrepo-backend/build/libs
```

# 运行MVNRepo

```shell
$ java -jar mvnrepo-backend-all.jar [-p 8000] [-t 114514]
```

> -p 指定的是端口号, 默认是8088  
> -t 指定的是admin默认账户的密码, 默认是随机UUID
> 只需要第一次启动的时候指定, 第一次启动完成后就会把密码记录进数据库
> 并且输出在控制台, 所以千万不要泄露出数据库文件

> 暂时没有前端页面(