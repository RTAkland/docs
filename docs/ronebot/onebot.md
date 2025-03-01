# ROneBot-onebot-v11

这里是OneBotV11 SDK的帮助文档

# 开始使用

框架需要使用`java-websocket`, `gson`, `kotlinx.coroutines` 三个库

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
    implementation("cn.rtast:ronebot-onebot-v11:${version}")
}
```

> 替换成最新版本, 最新版本可以在Maven仓库查看,
> [这里](https://repo.maven.rtast.cn/#/releases/cn/rtast/ronebot-onebot-v11)
> 是 所有版本的Maven仓库地址尽量使用最新版进行开发~ (你也可以查看仓库的tag, 因为每次有新的tag被推送进仓库就会自动触发Github
> Actions进行发布包)

### 快照版本

快照版本快照版本在每次commit之后都会生成，使用快照版本需要借助`jitpack.io`来实现

添加仓库

```kotlin
repositories {
    maven("https://jitpack.io")
}
```

添加依赖

```kotlin
dependencies {
    // 这里的版本替换成最新版本
    implementation("com.github.RTAkland:ROneBot:${version}")
}
```

使用快照版本

# 最小实例

```kotlin
fun main() {
    ROneBotFactory.createClient("ws://127.0.0.1:6666", "1145141919810", object : OneBotListener {
        override suspend fun onGroupMessage(message: GroupMessage, json: String) {
            println(message.rawMessage)
        }
    })
}
```

# 监听事件

ROB有两种事件分发系统

1. 实现`OneBotListener`
2. EventBus

> 从v2.8.0版本开始添加了`EventBus`事件分发系统, 你可以更方便的通过函数式编程的方式来监听事件

## 实现OneBotListener

```kotlin
class TestClient : OneBotListener {
    override suspend fun onGroupMessage(message: GroupMessage, json: String) {
        println(message)
    }

    override suspend fun onWebsocketErrorEvent(event: IWebsocketErrorEvent) {
        event.exception.printStackTrace()
    }

    // 重写其他你想要监听的方法
}
```

## EventBus

这种方法所有的事件Events在
[https://github.com/RTAkland/ROneBot/tree/main/ronebot-onebot-v11/src/main/kotlin/cn/rtast/rob/event/events](https://github.com/RTAkland/ROneBot/tree/main/ronebot-onebot-v11/src/main/kotlin/cn/rtast/rob/event/events)
文件夹内

```kotlin
// 首先你需要创建一个Bot实例
fun main() {
    val bot = ROneBotFactory.createClient(...)
    // onEvent方法需要传入一个泛型参数, 泛型类型就是上面
    // 文件夹内的类形, 所有的Event都附带一个action对象
    // 可以对Bot进行操作
    bot.onEvent<GroupMessageEvent> {
        println(it.action.getLoginInfo())
        println(it.message) // GroupMessage
    }
}
```

# 内置指令管理器

> 内置的指令管理器可以处理 `指令别名` 即多个指令名指向一个指令,

> ~~***注意***: 内置指令管理器使用空格来匹配第一个命令是否匹配,
> 执行所有命令都需要再命令名后加上一个空格才能被正确的执行
> 如果你有其他想法可以提交PR让框架直接实现你的想法让所有人都可以用
> 或者自行实现一个命令管理器~~

> ***注意: 原本的自带权限的命令管理器已被废弃, 且不可用***

```kotlin
/**
 * 带上匹配模式注解可以指定使用什么模式来匹配
 * 如果不写注解或者将注解设置成[MatchingStrategy.SPACES]
 * 的话默认使用空格来分割命令
 */
@CommandMatchingStrategy(MatchingStrategy.REGEX)
class EchoCommand : BaseCommand() {
    override val commandNames = listOf("/echo", "/eee")  // 指令别名写法
    override suspend fun executeGroup(listener: OneBotListener, message: GroupMessage, args: List<String>) {
        // args 参数默认去除掉了指令部分, 如果一条消息是 "/echo 114514 1919810" 那么args就是
        // listOf("114514", "1919810")
        message.action.sendGroupMessage(message.groupId, args.joinToString(" "))
    }
}

fun main() {
    val fancyBot = FancyBot()
    val instance1 = ROneBotFactory.createServer(6760, fancyBot)
    val commands = listOf<BaseCommand>(
        EchoCommand()
    )
    commands.forEach { ROneBotFactory.commandManager.register(it) }
}
```

# Brigadier指令管理器

> 使用Mojang开源的[Brigadier](https://github.com/Mojang/brigadier)库来注册和执行指令,
> 你可以按照以下代码来注册你的命令

```kotlin
class TestBrigadierCommand : BrigadierCommand() {
    private val scope = CoroutineScope(Dispatchers.IO)

    override fun register(dispatcher: CommandDispatcher<CommandSource>) {
        val root = LiteralArgumentBuilder.literal<CommandSource>("/foo")
            .then(
                // 这里的两个泛型参数一个是命令源(上下文固定写`CommandSource`), 另外一个是需要接收参数的类型
                RequiredArgumentBuilder.argument<CommandSource, String>("bar", StringArgumentType.string())
                    .executes {
                        // 这里必须catch所有的异常
                        scope.launch {
                            try {
                                // 这里是使用Brigadier提供的获取命令参数输入的方法
                                println(StringArgumentType.getString(it, "bar"))

                                // 这里是ROneBot拓展的获取命令参数输入的方法
                                println(it.getString("any"))
                            } catch (e: Exception) {
                                e.printStackTrace()
                            }
                        }
                        0
                    }
            )
        // 这里为`/foo`创建了一个重定向(指令别名)
        dispatcher.register(LiteralArgumentBuilder.literal<CommandSource>("/test").redirect(root.build()))
    }
}

suspend fun main() {
    // 省略上面的部分
    ROneBotFactory.brigadierCommandManager.register(TestBrigadierCommand())
}
```

> 注意上面示例代码中的泛型: `CommandSource` 的意义是下方`executes`函数的上下文类型, 注意不要写错了!

> 使用这个命令管理器注册的缺点就是 `executes`函数体是Java中的普通函数, 并**不是挂起函数***

> 由于在OneBot中消息有两种, 分别是: 群消息, 私聊消息. 所以`CommandContext`中提供了`messageType`枚举字段
> 以便判断消息类型, 在ROneBot中两种消息都实现了`IMessage`接口, 所以可以进行安全的强制类型转换, 如下所示

```kotlin
// ...
.executes { context ->
    scope.launch {
        (context.source.message as GroupMessage)?.reply("114514")
        (context.source.message as PrivateMessage)?.reply("114514")
    }
    0
}
```

> 当然`CommandContext`还提供了`privateMessage`和`groupMessage`字段, 但是这两个字段都可能为空
> (可空的意思是如果是群聊消息`privateMessage`就为空, 反之私聊消息`groupMessage`就为空)

> ***注意!!!注意!!!注意!!!*** 如果是用Brigadier注册的命令必须要在`executes`lambda内做好
> `try-catch`(捕获所有异常!!!), 不然的话一旦抛出异常那这个命令就没办法再次使用了!!!!(必须重启整个程序才能使用)

> 更多Brigadier的用法点[这里](https://github.com/Mojang/brigadier)

## 封装后的命令注册

如果你觉得上面那种方式太复杂还需要写一个类继承后再写指令，你可以选择下面的方式来注册命令

```kotlin
fun main() {
    ROneBotFactory.brigadierCommandManager.register(
        Commands.literal("main")
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

# 函数式命令处理器

这种方式可以让你更快速、简单的创建一个指令

```kotlin
// 注解内传入Array<String>用于表示命令的别名
@GroupCommandHandler(["/test"])
suspend fun testCommand(message: GroupMessage) {
    println(message)
}

@PrivateCommandHandler(["/t1"])
suspend fun privateCommand(message: PrivateMessage) {
    println(message)
}
```

> 这是群聊和私聊的指令实现方法, 你还需要注册这两个命令

```kotlin
fun main() {
    // 使用双冒号引用这个函数
    ROneBotFactory.commandManager.registerFunction(::testCommand)
    ROneBotFactory.commandManager.registerFunction(::privateCommand)
}
```

## 注意

> 这种方式使用到了反射来调用函数,
> 如果你的Bot大量使用到了这种方式请考虑使用继承的方式来实现你的指令

# 会话

> 在v2.8.2版本中引入了会话(Session)的API, 你可以轻松的创建一个对话而不用手写轮子，下面是一个简单的例子，
> 这里创建了一个群聊的测试指令: 在群聊中发送`/session 1`即可开始对话，
> 后续如果输入的不是`2`则会一直重复输入期间不会触发其他的命令包括这个对话命令本身。

```kotlin
class TestSession : BaseCommand() {
    override val commandNames = listOf("/session")

    override suspend fun executeGroup(message: GroupMessage, args: List<String>) {
        if (message.text.contains("1")) {
            message.reply("继续输入")
            // start表示开始对话
            message.startSession()
        }
    }

    override suspend fun onGroupSession(msg: GroupMessage) {
        if (msg.text.contains("2")) {
            // skip 表示结束对话
            msg.skipSession()
            msg.reply("设置成功")
        } else {
            // reject表示继续输入或者表示继续输入,
            // 按照使用情景不同可以有不同的意思,
            // 并附带一条消息
            msg.reject(messageChain {
                text("请输入2!")
            })
        }
    }
}
```

## 函数式命令处理器会话

```kotlin
class TestReceiver {
    @GroupSessionHandler
    suspend fun testGroup(message: GroupMessage) {
        println(message.action.getLoginInfo())
        if (!message.text.contains("2")) {
            rejectGroupSession(message, messageChain {
                text("请输入2")
            })
        } else {
            skipGroupSession(message)
            message.reply("设置成功")
        }
    }
}

@GroupCommandHandler(["/test"], TestReceiver::class)
suspend fun testCommand(message: GroupMessage) {
    // ::testCommand 是获取自己的函数引用对象
    startGroupSession(message, ::testCommand)
    message.reply("请继续输入2")
    println(message)
}
```

> 唯一的弊端就是注解只能传入`Class`的引用, 没办法传入顶层函数所以想要使用必须创建一个类作为接收器

# 消息构造器

## 链式调用构造消息

> 框架支持使用链式调用的方式构造一个消息, 以下是一个简单的示例, 也可以直接使用字符串的形式发送

```kotlin
val msgChain = MessageChain.Builder()
    .addAt(message.sender.userId)
    .addText(message.rawMessage)
    .addNewLine(3)  // 添加3个换行符到末尾
    .build()
this.sendGroupMessage(message.groupId, msgChain)
```

## 操作符重载构造消息

> 由于一些奇怪的特性, 直接使用 `+` 操作符对两个对象进行操作时
> IDE并不会有代码补全的提示, 也不会自动进行导入, 所以在某些情况下
> 你需要使用`XXX().plus`的形式进行导入这个拓展操作符

下面是示例代码

```kotlin
fun main() {
    // 可以直接对一个链式调用构造的消息使用操作符重载
    val chain = MessageChain.Builder().addText("1").build() + Text("")
    println(chain)
    val operator = AT(3458671395) +
            Text("114514") +
            Image("https://example.com/example.png") +
            Reply(114514L) +
            Face(666) +
            NewLine()
    println(operator)
}
```

## DSL消息构造

> 这一部分是Kotlin特有的调用方式看下面的示例

```kotlin
fun main() {
    val msg = messageChain {
        addText("Hello World")
        this(Text("1111"))
        invoke(Text("2222"))
        +Text("22222")
        add(Text("22222"))
        // 也可以像下面这样追加消息段
        text {
            text = "1111"
        }
        image {
            file = "<这里是base64>"
            base64 = true
        }
    }
    println(msg)
}
```

当然也可以使用`json-like`的方式创建一个合并转发消息或者普通消息
下面是合并转发消息的用法

```kotlin
val node = nodeMessageChain {
    messageChain(3458671395) {
        text("1111")
        text {
            text = 1
        }
        text {
            text = 2
        }
    }
    messageChain(3458671395) {
        text("2222")
    }
}
```

> 普通的消息就只需要去除外面一层的`nodeMessageChain`即可

# 消息即对象

> 你可以对一个消息对象执行某些操作例如 撤回(revoke/recall) 回复(reply)

```kotlin
fun main() {
    ROneBotFactory.createClient("ws://127.0.0.1:6666", "1145141919810", object : OneBotListener {
        override suspend fun onGroupMessage(message: GroupMessage, json: String) {
            message.revoke(10)  // 延迟10秒后撤回这条消息
            // 你可以在任何事件对象中访问`action`, 例如下面的示例
            println(message.action.getLoginInfo())
        }
    })
}
```

# 内置任务调度器

> 你可以创建自己的任务并设置延迟时间和循环周期

```kotlin
fun main() {
    val instance1 = ROneBotFactory.createClient("ws://127.0.0.1:6666", "1145141919810", object : OneBotListener {
        override suspend fun onGroupMessage(message: GroupMessage, json: String) {
            message.revoke(10)  // 延迟10秒后撤回这条消息
        }
    })
    instance1.scheduler.scheduleTask({
        println(it.action.getLoginInfo())
    }, 0, 1000)
    taskHandle.cancel()  // 取消这个任务
    instance1.scheduler.cancelTask(taskHandle)  // 或者这样写
}
```

# Deferred对象操作

> 虽然Websocket所有操作都是异步执行, 但是得益于Kotlin协程的`CompletableDeferred<T>>()`
> 你可以像调用普通函数一样调用一些有返回值的异步操作参考以下例子

```kotlin
fun main() {
    ROneBotFactory.createClient("ws://127.0.0.1:3001", "114514", object : OneBotListener {
        override suspend fun onGroupMessage(message: GroupMessage, json: String) {
            val versionInfo = this.getLoginInfo()  // 这里使用了CompletableDeferred来操作
            println(versionInfo)
        }
    })
}
```

# 权限控制

见[Permission](docs/ronebot/permission.md)

# 合理使用反射和注解

> 反射可以在程序运行时反观或修改一个类的结构. 注解可以在不修改类结构的同时为类增加新的行为和属性

> 默认指令管理器只提供了`指令别名`如果你想给一个命令添加一个描述你可以使用注解+反射来实现

假设你将所有的命令存储在了一个list中

```kotlin
// 仅作演示 HelpCommand被定义在下面
val commands = listOf<BaseCommand>(HelpCommand())
```

现在定义一个注解

```kotlin
@Target(AnnotationTarget.CLASS)
@Retention(AnnotationRetention.RUNTIME)
annotation class CommandDescription(val description: String)
```

> 这个注解包含了一个`description`属性来提供这个指令的描述

再来定义一个Help指令

```kotlin
@CommandDescription("帮助")
class HelpCommand : BaseCommand() {
    override val commandNames = listOf("/help")

    override suspend fun executeGroup(listener: OneBotListener, message: GroupMessage, args: List<String>) {
        val msg = MessageChain.Builder()
        commands.sortedBy { it::class.simpleName }.forEach {
            // 这里使用反射来获取所有指令的注解, 如果一个类没有被`CommandDescription`注解就设置成`暂无描述`
            val description = if (!it::class.hasAnnotation<CommandDescription>()) "暂无描述"
            else it::class.findAnnotation<CommandDescription>()?.description!!
            // 获取每个命令的类名
            val commandName = it::class.simpleName!!
            // 将每个指令别名合并成一个字符串
            val commandNames = it.commandNames.joinToString(",")
            // 添加到最终的输入消息中
            msg.addText("[$commandName] [$description] 命令: $commandNames").addNewLine()
        }
        msg.addText("共计${commands.size}条命令")
        message.reply(msg.build())
    }
}
```

> 现在发送`/help`就可以看到效果了


***注意*** 反射是一种耗时的操作需要合理的运用反射才能让程序获得更好的性能

如果你还想自定义指令的名称还可以自己新建一个注解来为你的指令添加自定义指令名
只需要修改构造消息的部分就可以完成

# 自定义命令拦截器

这个功能可以实现在命令执行之前/之后执行某些代码，
需要使用这个功能可以按照下面的方式创建并且注册一个
自定义Interceptor

```kotlin
class CustomInterceptor : ExecutionInterceptor() {
    override suspend fun beforeGroupExecute(message: GroupMessage, command: BaseCommand): CommandExecutionResult {
        if (xxxx) {
            return CommandExecutionResult.STOP
        }
        return CommandExecutionResult.CONTINUE
    }
}

fun main() {
    ROneBotFactory.interceptor = CustomInterceptor()
}
```

# 消息序列化器

> 对一个`List<ArrayMessage>`列表使用`.serialize()`方法可以将其序列化成具体的消息段数据类对象

```kotlin
class TestClient : OneBotListener {
    override suspend fun onGroupMessage(message: GroupMessage, json: String) {
        // 或者使用`.toSegments()`方法
        message.message.serialize().apply {
            // has 方法用于判断消息段列表是否有某种消息段类型
            println(has(TextSegment::class))
        }.forEach {
            // 使用 is 关键字智能类型转换为具体的消息段类型
            if (it is TextSegment) {
                println(it.text)
            }
        }
    }
}
```