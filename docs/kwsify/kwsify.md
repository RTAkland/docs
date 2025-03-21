# 下面是所有数据包的偏移值表

这里是所有的OP值

| OP值 | 说明     | 发送方 |
|-----|--------|-----|
| 0   | 发布消息   | 客户端 |
| 1   | 系统广播消息 | 服务端 |
| 2   | 接收消息   | 服务端 |
| 3   | 订阅频道   | 客户端 |
| 4   | 退订频道   | 客户端 |
| 5   | 心跳包    | 客户端 |
| 6   | 心跳包回复  | 服务端 |

> 所有数据包的前8+任意字节数都是固定格式，
> 下面是所有数据报的开头的固定格式

| 数据包         | 偏移值 | 说明                 | 类型   |
|-------------|-----|--------------------|------|
| op          | 0-4 | 用作op操作符            | Int  |
| channelSize | 4-8 | 用于表示channel占用了多少字节 | Int  |
| channel     | 8-x | channel的名称         | Byte |

# 发布消息(0)

| 数据包      | 偏移值   | 说明          | 类型   |
|----------|-------|-------------|------|
| bodySize | x+4   | 用于表示Body的长度 | Int  |
| body     | x+4+y | body的内容     | Byte |

# 订阅频道(3)

| 数据包           | 偏移值     | 说明              | 类型      |
|---------------|---------|-----------------|---------|
| broadcastSelf | x+1     | 表示是否接收来自自己发布的消息 | Boolean |
| uuid高位        | x+1+8   | uuid的高位         | Long    |
| uuid低位        | x+1+8+8 | uuid的低位         | Long    |

# 退订频道(4)

| 数据包         | 偏移值 | 说明                 | 类型   |
|-------------|-----|--------------------|------|
| op          | 0-4 | 用作op操作符            | Int  |
| channelSize | 4-8 | 用于表示channel占用了多少字节 | Int  |
| channel     | 8-x | channel的名称         | Byte |

# 接收消息(2, 1)

| 数据包         | 偏移值                 | 说明                 | 类型   |
|-------------|---------------------|--------------------|------|
| op          | 0-4                 | 用作op操作符            | Int  |
| channelSize | 4-8                 | 用于表示channel占用了多少字节 | Int  |
| channel     | 8-x                 | channel的名称         | Byte |
| bodySize    | x+4                 | 用于表示Body的长度        | Int  |
| body        | x+4+y               | body的内容            | Byte |
| hostSize    | x+4+y+4             | 发送者主机名的长度          | Int  |
| host        | x+4+y+4+z           | 发送者主机名             | Byte |
| port        | x+4+y+4+z+4         | 发送者的端口号            | Int  |
| addressSize | x+4+y+4+z+4+a       | 发送者地址的长度           | Int  |
| address     | x+4+y+4+z+4+a+b     | 发送者地址              | Byte |
| uuid高位      | x+4+y+4+z+4+a+b+8   | uuid的高位            | Long |
| uuid低位      | x+4+y+4+z+4+a+b+8+8 | uuid的低位            | Long |

# 心跳包(5)

| 数据包 | 偏移值 | 说明      | 类型  |
|-----|-----|---------|-----|
| op  | 0-4 | 用作op操作符 | Int |

# 心跳包回复(6)

> 和心跳包格式一样, 不过只能由服务端发送