<div align="center">

<img src="https://static.rtast.cn/static/icon/ChatC-icon.png" alt="ChatC-ICON" width="150">

<h2>ChatConnector</h2>

<h3>⭐一个可以互通QQMC消息以及子服消息同步的Velocity插件⭐</h3>

<img src="https://img.shields.io/badge/OneBot-v11-black?logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAHAAAABwCAMAAADxPgR5AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAAxQTFRF////29vbr6+vAAAAk1hCcwAAAAR0Uk5T////AEAqqfQAAAKcSURBVHja7NrbctswDATQXfD//zlpO7FlmwAWIOnOtNaTM5JwDMa8E+PNFz7g3waJ24fviyDPgfhz8fHP39cBcBL9KoJbQUxjA2iYqHL3FAnvzhL4GtVNUcoSZe6eSHizBcK5LL7dBr2AUZlev1ARRHCljzRALIEog6H3U6bCIyqIZdAT0eBuJYaGiJaHSjmkYIZd+qSGWAQnIaz2OArVnX6vrItQvbhZJtVGB5qX9wKqCMkb9W7aexfCO/rwQRBzsDIsYx4AOz0nhAtWu7bqkEQBO0Pr+Ftjt5fFCUEbm0Sbgdu8WSgJ5NgH2iu46R/o1UcBXJsFusWF/QUaz3RwJMEgngfaGGdSxJkE/Yg4lOBryBiMwvAhZrVMUUvwqU7F05b5WLaUIN4M4hRocQQRnEedgsn7TZB3UCpRrIJwQfqvGwsg18EnI2uSVNC8t+0QmMXogvbPg/xk+Mnw/6kW/rraUlvqgmFreAA09xW5t0AFlHrQZ3CsgvZm0FbHNKyBmheBKIF2cCA8A600aHPmFtRB1XvMsJAiza7LpPog0UJwccKdzw8rdf8MyN2ePYF896LC5hTzdZqxb6VNXInaupARLDNBWgI8spq4T0Qb5H4vWfPmHo8OyB1ito+AysNNz0oglj1U955sjUN9d41LnrX2D/u7eRwxyOaOpfyevCWbTgDEoilsOnu7zsKhjRCsnD/QzhdkYLBLXjiK4f3UWmcx2M7PO21CKVTH84638NTplt6JIQH0ZwCNuiWAfvuLhdrcOYPVO9eW3A67l7hZtgaY9GZo9AFc6cryjoeFBIWeU+npnk/nLE0OxCHL1eQsc1IciehjpJv5mqCsjeopaH6r15/MrxNnVhu7tmcslay2gO2Z1QfcfX0JMACG41/u0RrI9QAAAABJRU5ErkJggg==">
<img src="https://img.shields.io/badge/Kotlin-2.0.10-pink?logo=kotlin">

</div>

# 概述

这个插件可以互通QQ和MC的消息, 包括不限于纯文本、同步子服之间的消息(跨服聊天)、远程执行命令
快速添加白名单, 解析消息段并转换成游戏内支持的文本格式(可以点击消息、悬浮文本预览内容), 并且支持将其他子服的玩家显示到玩家列表中

如果你需要任何帮助你可以加入ChatConnector交流群(575300987), 有任何问题都可以在这里面问, 前提是你读完并且了解了本文档。
我会在这个群内测试我的插件

# 展示

<div style="display: flex;">
    <img src="../images/chatc/chat-show.png" alt="Image 1" style="width: 50%; height: auto;">
    <img src="../images/chatc/qq-chat-show.png" alt="Image 2" style="width: 50%; height: auto;">
</div>

![bStats](https://bstats.org/signatures/velocity/ChatConnector.svg)

# 使用

!> v3.x已经弃用了Discord/Kook的支持

> 这里不赘述配置OneBot11的实现, 只需要找到一个支持正向Websocket的OneBot11实现  
> 这里给出几个可以用的实现: `Lagrange.OneBot` `Napcat` `LlOneBot`

***注意: 除了Lagrange.OneBot其余都没有测试过, 请自行测试可用性***

## 已支持解析的消息类型

|      功能/平台      | MC向QQ | QQ向MC |
|:---------------:|:-----:|:-----:|
|       纯文本       |   ✅   |   ✅   |
|      通用CQ码      |   ✅   |       |
|       图片        |       |   ✅   | 
|       语音        |       |   ✅   |  
|       艾特        |       |   ✅   |
|      消息链转发      |       |   ✅   |
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

## 支持的功能

1. 支持在Tab列表中显示出其他子服的玩家名，并且将其他子服的玩家名字设置成灰色
2. 跨服聊天 - 其他子服发的消息可以广播到全部子服
3. 群服互通， 将QQ群和MC的消息相互转发, 也可以设置游戏内转发的前缀, 必须以某个前缀开头的消息才会被转发到QQ群, 默认为任何消息都转发
4. QQ群内执行MC的命令
5. 操作白名单
6. 多QQ群转发 - 多个QQ群的消息转发到MC，MC的消息同样会被转发到所有在配置文件内的群中

## 指令

### 游戏内指令

> 游戏内的指令都是/chatc开头的

可用的游戏内指令如下

1. /chatc reconnect - 重连Websocket和Rcon

### 群内指令

> 群内的指令前缀可以为一下三种的任意一个

1. !!
2. ！！
3. /

> 意思就是如果一个命令是`help` 那么你可以在群内发送下面三种情况中的任意一种就能成功执行

1. !!help
2. ！！help
3. /help

可用的群内指令如下

1. !!at <玩家名>  在群内@游戏内的玩家(不论在哪个子服, 只要是通过Velocity连接到的玩家都能@), 被@的玩家会在屏幕上提示(
   但是没有声音, 因为这是Velocity的限制)
2. !!exec <rcon名字> <指令>  在群内对子服执行命令, 需要配置RCOn
3. !!help 帮助
4. !!list 列出在线玩家
5. !!reconnect 重连Websocket和RCON
6. !!status 获取服务器状态
7. !!wh <on | off | add | remove> [玩家名] 操作白名单

## 配置

```json
{
   // 密钥
  "secretKey": "<your secret key here>",
   // 正向Websocket连接地址
  "wsAddress": "ws://127.0.0.1:8081/ws",
  "accessToken": "1145141919810",
   // 表示监听的群聊， 必须是真实存在并且机器人在群内的， 如果你只需要监听一个群的话，那就把其他的全部删掉
  "groupId": [
    114514,
    1919810
  ],
   // 表示需要上报的事件， 不需要哪个事件就删除掉哪一行
  "events": [
    "PlayerLeaveEvent",  // 玩家退出游戏
    "PlayerJoinEvent",  // 玩家加入游戏
    "PlayerChatEvent",  // 玩家聊天消息
    "GroupMessageEvent",  // 从QQ群转发消息
    "CrossServerMessageEvent"  // 跨服聊天消息
  ],
   // 权限配置, owner和admins权限一样， others表示没权限， 可以不填
  "permission": {
    "owner": 3458671395,
    "admins": [
      114514,
      1919810
    ],
    "others": [
      66666,
      33343131
    ]
  },
   // 配置RCON
  "rcons": {
     // 是否启用RCON， 如果需要操作白名单的话就需要开启并正确配置
    "enabled": false,
    "rcons": [
      {
        "name": "instance1",
        "host": "127.0.0.1",
        "port": 25577,
        "password": "123456"
      },
      {
        "name": "instance2",
        "host": "127.0.0.1",
        "port": 25599,
        "password": "1919810"
      }
    ]
  },
  "style": {
     // 跨服聊天的消息样式， 遵循MiniMessage语法
    "crossServerMessageStyle": "<green>[{{serverName}}]</green> > [{{playerName}}]: {{message}}",
     // QQ群转发来的消息的样式， 遵循MiniMessage语法
    "groupMessageStyle": "<green>[QQ群({{groupName}})]</green> > [{{senderName}}]: {{message}}"
  },
  "mcForwardPrefix": "",  // 这里是控制以什么前缀开头的消息会被转发到QQ群, 设置成空就是任何消息都会被转发到QQ群
  "enableTabList": true  // 是否开启在Tab列表显示其他子服的玩家
}
```

这是默认的配置文件, 你可以按需修改, 如果不知道怎么配置的话可以联系我帮忙配置

# 注

!> 因为QQ官方机器人API限制了主动发送消息次数(每个月限制4条消息)，所以暂时不支持QQ官方机器人API消息处理器

!> 本插件支持多群组互通即: 多对一 (两个以上QQ群转发MC内的消息, MC的消息转发到所有QQ群), 但是不支持不同平台的消息监听

!> 如果想要增加新的功能，则需要加价最少20元，会按照难度来决定是否增加这个价格

!> 如果你非常非常懒一点都不想自己配置，那你需要额外付15元作为技术费，
!> 我会全程帮你部署所有的东西, 如果你只是有一小部分不懂或者不会那么我会考虑免费帮你

# 购买

!> 请通过[邮件](mailto:buy@rtast.cn)联系我并进行购买，我会尽可能快的回复您的消息 。 也可以直接通过QQ联系我 `3458671395`。
本插件定价暂时为 `20元(RMB)`。

# 许可协议

本项目使用CLA协议, 即:

- 明确禁止反编译、分发等行为。
- 声明软件的知识产权归我所有。
- 免责声明，软件按现状提供。
- 这是双方之间的完整协议。

> 下方是CLA协议文件原文

```license
Commercial License Agreement

This Commercial License Agreement (the "Agreement") is entered into as of [Date] by and
between [Your Name or Your Company Name] (the "Licensor") and [Licensee's Name] (the "Licensee").

1. Grant of License

Licensor hereby grants Licensee a non-exclusive, non-transferable, revocable license to use the software product known
as [Your Software Name] (the "Software") for [Purpose of Use, e.g., personal use, internal business use] under the terms
of this Agreement.

2. Restrictions

Licensee shall not:

Decompile, reverse engineer, disassemble, or otherwise attempt to derive the source code of the Software.
Modify, adapt, or create derivative works based on the Software.
Distribute, sell, lease, or sublicense the Software or any part thereof to any third party.
Use the Software in any manner that violates applicable laws or regulations.

3. Ownership

Licensor retains all rights, title, and interest in and to the Software, including all intellectual property rights.

4. Term and Termination

This Agreement shall commence on the date first written above and continue until terminated by either party. Licensor
may terminate this Agreement immediately if Licensee breaches any terms of this Agreement. Upon termination, Licensee
shall cease all use of the Software and destroy all copies in their possession.

5. No Warranty

The Software is provided "as is," without warranty of any kind, express or implied, including but not limited to the
warranties of merchantability or fitness for a particular purpose.

6. Limitation of Liability

Licensor shall not be liable for any damages arising from the use of or inability to use the Software, including but not
limited to consequential, incidental, or punitive damages.

7. Governing Law

This Agreement shall be governed by and construed in accordance with the laws of [Your State/Country].

8. Entire Agreement

This Agreement constitutes the entire agreement between the parties and supersedes all prior agreements and
understandings with respect to the subject matter hereof.

IN WITNESS WHEREOF, the parties have executed this Commercial License Agreement as of the date first written above.

Licensor:
Name: [RTAkland]

Licensee:
Name: [CLA]
```