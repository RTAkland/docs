# Permission

目前权限控制仅实现了`OneBot11`平台, 你可以在Brigadier和旧版注册的命令中使用

## 添加Maven仓库

```kotlin
repositories {
    // RTAST的Maven仓库
    maven("https://repo.maven.rtast.cn/releases/")
}
```

## 添加依赖

```kotlin
dependencies {
    // 这里的版本替换成最新版本
    implementation("cn.rtast:ronebot-permission:${version}")
}
```

> 替换成最新版本, 最新版本可以在Maven仓库查看,
> [这里](https://repo.maven.rtast.cn/#/releases/cn/rtast/ronebot-permission)
> 是 所有版本的Maven仓库地址尽量使用最新版进行开发~ (你也可以查看仓库的tag, 因为每次有新的tag被推送进仓库就会自动触发Github
> Actions进行发布包)

# Brigadier 

```kotlin

suspend fun main() {
    ROneBotFactory.getPermissionManager().apply {
//        setUserPermissionLevel(3458671395.toString(), BasicPermission.User)
//        setUserPermission(3458671395.toString(), BasicPermission.User)
//        setUserPermission(3458671395.toString(), "command.test.main")
        // 只要大于3就拥有所有权限, 所有权限指的是BasicPermission和Int level的所有权限
        // 权限节点需要单独配置
        setUserPermission(3458671395.toString(), 114514)

    }
    ROneBotFactory.brigadierCommandManager.register(
        Commands.literal("main")
//            .requires { it.hasPermission(BasicPermission.Admin) }
//            .requires { it.hasPermission("command.test.main") }
            .requires { it.hasPermission(114514) }
            .then(
                Commands.argument("test", CharArgumentType.char())
                    .executes {
                        println(it.getChar("test"))
                        Command.SINGLE_SUCCESS
                    }
            ), listOf("main1", "1111")
    )
}
```

# 旧版命令

> 旧版命令中你只需要使用`hasPermission`这个多个类型的重载函数然后判断返回值就能判断是否有权限了