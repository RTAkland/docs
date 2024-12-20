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

# 发布包

在build.gradle.kts中添加以下内容

```kotlin
publishing {
    // 其他配置
    repositories {
        maven {
            url = uri("http://127.0.0.1:8088/releases")
            isAllowInsecureProtocol = true
            credentials {
                username = "admin"
                password = "<xxxxxx>"
            }
        }
    }
}
```

# 下载包

```kotlin
repositories {
    maven("http://127.0.0.1:8088/releases")
}

dependencies {
    implementation("cn.rtast:test-publish-4:0.0.1")
}
```

# 注意事项

现在有`releases` 和 `snapshots`是公开仓库 `private`是私有仓库, 发布和下载包都需要认证, 通过API访问私有包也需要认证