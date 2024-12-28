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

## 配置

```json
{
  "secretKey": "<your secret key here>",
  "wsAddress": "ws://127.0.0.1:8081/ws",
  "accessToken": "1145141919810",
  "groupId": [
    114514,
    1919810
  ],
  "events": [
    "PlayerLeaveEvent",
    "PlayerJoinEvent",
    "PlayerChatEvent",
    "GroupMessageEvent",
    "CrossServerMessageEvent"
  ],
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
  "rcons": {
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
    "crossServerMessageStyle": "<green>[{{serverName}}]</green> > [{{playerName}}]: {{message}}",
    "groupMessageStyle": "<green>[QQ群({{groupName}})]</green> > [{{senderName}}]: {{message}}"
  },
  "mcForwardPrefix": "",
  "enableTabList": true
}
```

这是默认的配置文件, 你可以按需修改, 如果不知道怎么配置的话可以联系我帮忙配置

# 注

!> 因为QQ官方机器人API限制了主动发送消息次数(每个月限制4条消息)，所以暂时不支持QQ官方机器人API消息处理器

!> 本插件支持多群组互通即: 多对一 (两个以上QQ群转发MC内的消息, MC的消息转发到所有QQ群), 但是不支持不同平台的消息监听

# 购买

!> 请通过[邮件](mailto:buy@rtast.cn)联系我并进行购买，我会尽可能快的回复您的消息 。 也可以直接通过QQ联系我 `3458671395`。
本插件定价暂时为 `10元(RMB)`。

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