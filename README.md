<div align="center">

<img src="https://static.rtast.cn/static/icon/ChatC-icon.png" alt="ChatC-ICON" width="150">

<h2>ChatConnector</h2>

<h3>⭐一个可以互通QQ/Discord/Kook和MC消息以及子服消息同步的Velocity插件⭐</h3>

<img src="https://static.rtast.cn/static/kotlin/made-with-kotlin.svg" alt="MadeWithKotlin">

</div>

# 概述

这个插件可以互通QQ/Kook/Discord和MC的消息, 包括不限于纯文本、同步子服之间的消息(跨服聊天)、远程执行命令
快速添加白名单(格式: `ID:xxx`), 解析CQ码并转换成游戏内支持的文本格式(可以点击消息、悬浮文本预览内容)

> 如果你需要其他Mod加载器或其他平台的插件请通过[邮件](mailto:rtakland@outlook.com)
> 联系我并进行购买，我会尽可能快的回复您的消息。

![bStats](https://bstats.org/signatures/velocity/ChatConnector.svg)

# 使用

## OneBot消息处理器

此插件对接`Lagrange.OneBot`框架你需要前往[Lagrange](https://github.com/LagrangeDev/Lagrange.Core/)  
下载最新的Lagrange并配置正向Websocket和正向HTTP 也可以按需配置AccessToken
下载最新的jar文件放入你的Velocity插件文件夹内  
然后~~原神, 启动!~~

> 因为使用的是OneBotV11规范所以理论上大部分OneBotV11实现都可以使用此插件

## 配置OneBotV11实现

### 已支持解析的消息类型

|      功能/平台      | MC向QQ | QQ向MC |
|:---------------:|:-----:|:-----:|
|       纯文本       |   ✅   |   ✅   |
|      通用CQ码      |   ✅   |       |
|       图片        |       |   ✅   | 
|       语音        |       |   ✅   |  
|       艾特        |       |   ✅   |
|      消息连转发      |       |   ✅   |
|       位置        |       |   ✅   |
|       收藏        |       |   ✅   |
|      好友推荐       |       |   ✅   |
|      群聊推荐       |       |   ✅   |
|       文档        |       |   ✅   |
|      音乐分享       |       |   ✅   |
|      班级作业       |       |   ✅   |
|   视频分享(哔哩哔哩)    |       |   ✅   |
|       骰子        |       |   ✅   |
|      石头剪刀布      |       |   ✅   |
|    群公告(不含图片)    |       |   ✅   |
| MiniMessage语法消息 |       |   ✅   |

### 配置Lagrange.OneBot

你需要前往[Lagrange](https://github.com/LagrangeDev/Lagrange.Core/)仓库下载最新的Lagrange.OneBot, 并登录你的账号
向`appsettings.json`文件中的`Implementations`设置为

```json
[
  {
    "Type": "ForwardWebSocket",
    "Host": "*",
    "Port": 8081,
    "HeartBeatInterval": 5000,
    "HeartBeatEnable": true,
    "AccessToken": "114514114514"
  },
  {
    "Type": "Http",
    "Host": "*",
    "Port": 8083,
    "AccessToken": "114514114514"
  }
]
```

你可以按需填写`AccessToken`键, 然后记住你的配置文件中的内容, ***请确保两个AccessToken一致***

### 注意事项

> 一个OneBotV11实现可以对应多个群例如有一个机器人, 两个群并且
> 这两个群都加入了, 两个MC服务器则需要在Velocity配置文件中的
> groupId修改成需要监听的QQ群

> Websocket的连接会在初始化插件的时候自动进行连接, 不需要手动连接
> 当非正常关闭链接的时候自动重连

## Kook消息处理器

Kook中暂时只支持纯文本以及部分emoji表情的解析/收发

> 如何才能找到你的聊天频道的ID呢? 答: 下面这张图箭头指向的就是你的频道ID位于最后一个斜杠后, 双击复制即可
![channel_id](https://static.rtast.cn/images/kook_channel.png)

打开`config.json`将复制下来的id填入`groupId`内

> 那AccessToken如何获取呢? 答: 去开发者平台注册一个Bot 点击[这里](https://developer.kookapp.cn/app/index)
> 快速注册一个Bot, 你需要登陆后点击右上角`新建应用`输入一个名称, 然后点击新建的Bot的头像, 点击左侧侧边栏的`机器人`
> 在`机器人连接模式`下可以找到`Token`字样, 这就是AccessToken了复制下来将其填入`config.json` 内的`AccessToken`
> 消息处理器

## Discord消息处理器

Discord 中只支持纯文本以及部分emoji表情解析

由于某种原因使用Discord消息处理器需要使用代理才能正常使用(如果你在国内)

你需要将`messageHandler`修改成`Discord`然后accesstoken填你的Bot的Token再配置一个HTTP代理，
最后将`groupId`修改成你的频道ID就可以了

# 配置Velocity

在`plugins/ChatConnector`文件夹内找到`config.json`默认配置文件如下:

<details>
<summary>点击这里展开默认的配置文件</summary>

```json
{
  "secretKey": "<your secret key here>",
  "lang": "zh_cn",
  "wsAddress": "ws://127.0.0.1:8081/ws",
  "httpAddress": "http://127.0.0.1:8083",
  "accessToken": "114514",
  "messageHandler": "OneBot",
  "groupId": 114514,
  "events": [
    "InitEvent",
    "PlayerLeaveEvent",
    "PlayerJoinEvent",
    "PlayerChatEvent"
  ],
  "permission": {
    "owner": 114514,
    "admins": [
      114514,
      1919810
    ],
    "others": [
      1111111,
      1111
    ]
  },
  "rcons": {
    "enabled": false,
    "rcons": [
      {
        "name": "test",
        "host": "127.0.0.1",
        "port": 25577,
        "password": null
      },
      {
        "name": "test",
        "host": "127.0.0.1",
        "port": 25599,
        "password": null
      }
    ]
  },
  "proxy": {
    "host": "localhost",
    "port": 7890
  },
  "commands": {
    "execPermission": 2,
    "whitelistPermission": 2,
    "shortWhitelistPermission": 1,
    "listPermission": 1,
    "statusPermission": 1
  }
}
```

</details>

> 其中secretKey是你购买的时候我发方法给你的密钥, 请不要将密钥共享给他人, 因为你共享给别人用你自己就会被挤下线  
> wsAddress是Lagrange的ws服务器地址, httpAddress是Lagrange的Http服务器地址

> lang是多语言, 可选的选项有`zh_cn` `en_us` 目前仅支持这些语言

> accessToken是访问密钥, 在Lagrange中可以设置, 你必须将Ws和http的accessToken设置为相同的值
> 如果你使用了Kook或者Discord你需要注册一个App, Kook注册应用可以点[这里](https://developer.kookapp.cn/app/index)
> 快速注册

> messageHandler是消息处理器类型目前支持 `OneBot(QQ机器人)` `Kook(开黑啦)` `Discord`, 可选的值分别是
> `OneBot` `Discord` `Kook` 请务必注意大小写

> groupId是需要监听消息的QQ群号, 如果你用的是Kook或者Discord你需要将其设置为文字频道的频道号

> events是监听的事件类型, 这里是`Event`类型 `PlayerJoinEvent` `PlayerLeaveEvent` `PlayerChatEvent` `InitEvent`
> PlayerJoinEvent表示玩家加入的事件如果添加了这一行则表示玩家加入服务器则会向qq群发送消息
> PlayerLeaveEvent表示玩家离开服务器的事件,PlayerChatEvent表示玩家聊天事件(这个聊天事件指的是MC里的聊天事件),
> InitEvent表示是否在插件初始化完成后在群内发送`初始化成功xxxxx`字样的信息

> permission表示对应QQ号拥有的级别

> rcons表示后端服务器的RCON地址, 如果你需要执行除`!!status` `!!list`以外的命令则需要配置rcon

> commands表示每个命令所需要的权限的ID, `Owner=3` `Admin=2` `Other=1(所有人都可以执行)`
> commands.enableShortWhitelistCommand则表示是否开启简短的白名单添加方式, 格式为 `ID:xxx`

# 命令

使用`/chatc` 来获取所有的子命令

> `/chatc reload` 重载配置文件  
> `/chatc reconnect` 重新连接Websocket服务器  
> `/chatc disconnect` 断开Websocket的连接
> `/chatc reload ws` 重新连接Websocket(在更改了配置文件的情况下重连需要使用此命令)

# 注

> 如果你需要使用frp将ws服务器映射到公网请使用http协议并设置accesstoken

> 如果你想在一个端口上同时使用http和websocket并配置ssl可以参考以下nginx的配置文件
<details>
<summary>点击展开配置文件</summary>

```conf
server {
    listen 443 ssl;
    server_name bot.example.com;

    ssl_certificate cert.pem;
    ssl_certificate_key key.pem;

    # HTTP
    location / {
        proxy_pass http://localhost:8083/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # Websocket
    location /ws {
        proxy_pass http://localhost:8081/;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # Additional SSL settings for security
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_prefer_server_ciphers on;
    ssl_ciphers ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
    ssl_session_tickets off;
}

```

</details>

# 许可协议

本项目使用CLA协议, 即:

- 明确禁止反编译、分发等行为。
- 声明软件的知识产权归我所有。
- 免责声明，软件按现状提供。
- 这是双方之间的完整协议。

你可以在[这里](./licenses/CLA)查看完整的许可证文件
