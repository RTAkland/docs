# Permission

目前权限控制近实现了`OneBot11`平台, 你可以在Brigadier和旧版注册的命令中使用

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